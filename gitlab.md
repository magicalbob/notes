# To get on to the gitlab server:

	kubectl exec -ti service/gitlab -n gitlab -- bash

	then run `gitlab-rails console`

# Create new root user when one is missing:

	Create a new namespave (if needed):

		root_group = Namespace.create(name: 'root_group', path: 'root_group')

	Create new username root:
		root_user = User.new(username: 'root', email: 'root@example.com', password: 'password', password_confirmation: 'password', name: 'Root User', admin: true, namespace: root_group)
		root_user.save!

		*** password will not be accepted, make more difficult! ***

# Application Settings:

	app_setting = ApplicationSetting.current

# To stop gitlab so that you can do maintenance like restores etc:

	kubectl exec -it $(kubectl get pods -n gitlab -o jsonpath='{.items[0].metadata.name}') -n gitlab -- gitlab-ctl stop puma
	kubectl exec -it $(kubectl get pods -n gitlab -o jsonpath='{.items[0].metadata.name}') -n gitlab -- gitlab-ctl stop sidekiq

# To restore a backup of gitlab:

	# Create the backups directory if it doesn't exist
	kubectl exec -it $(kubectl get pods -n gitlab -o jsonpath='{.items[0].metadata.name}') -n gitlab -- mkdir -p /var/opt/gitlab/backups
	
	# Copy the latest backup file to the pod
	kubectl cp /vagrant/backups/1733386105_2024_12_05_17.5.2_gitlab_backup.tar gitlab/$(kubectl get pods -n gitlab -o jsonpath='{.items[0].metadata.name}'):/var/opt/gitlab/backups/

	# Set the correct ownership to the git user (will need to give some time for prev cp to complete)
	kubectl exec -it $(kubectl get pods -n gitlab -o jsonpath='{.items[0].metadata.name}') -n gitlab -- chown git:git /var/opt/gitlab/backups/1733386105_2024_12_05_17.5.2_gitlab_backup.tar

	# Run this command to do the restore
	kubectl exec -it $(kubectl get pods -n gitlab -o jsonpath='{.items[0].metadata.name}') -n gitlab -- gitlab-backup restore BACKUP=1733386105_2024_12_05_17.5.2
	# Add `SKIP=ci_runners` to the resore command to skip restoring gitlab runners
	# To truncate the runners on the db
	TRUNCATE TABLE ci_runners CASCADE;

	# After the restore, restart GitLab
	kubectl exec -it $(kubectl get pods -n gitlab -o jsonpath='{.items[0].metadata.name}') -n gitlab -- gitlab-ctl restart

# To start gitlab post maintenance

	kubectl exec -it $(kubectl get pods -n gitlab -o jsonpath='{.items[0].metadata.name}') -n gitlab -- gitlab-ctl restart

# Projects

## Create a blank project:

	curl --request POST "https://gitlab.ellisbs.co.uk/api/v4/projects" \
	     --header "PRIVATE-TOKEN: ${SELF_GITLAB_TOKEN}" \
	     --header "Content-Type: application/json" \
	     --data '{
	       "name": "check_all_schedules",
	       "visibility": "public"
	     }'

## List all projects:

	curl --request GET \
	     --header "PRIVATE-TOKEN: $SELF_GITLAB_TOKEN" \
             "https://gitlab.ellisbs.co.uk/api/v4/projects" |
             jq ".[].name"

	The `jq ".[].name"` returns just the repo names.

## Project Schedules

### List a particular projects schdules:

	curl --request GET \
	     --header "PRIVATE-TOKEN: $SELF_GITLAB_TOKEN" \
	     "https://gitlab.ellisbs.co.uk/api/v4/projects/6/pipeline_schedules" | jq .

### List all pipeline schedules:

	This builds on "List all projects". It gets the Id's of all the projects then uses xargs to get the schdules for each project id.

	curl --request GET \
             --header "PRIVATE-TOKEN: $SELF_GITLAB_TOKEN" \
             "https://gitlab.ellisbs.co.uk/api/v4/projects" |
             jq ".[].id"|xargs -I{} curl --request GET \
             --header "PRIVATE-TOKEN: $SELF_GITLAB_TOKEN" \
             "https://gitlab.ellisbs.co.uk/api/v4/projects/{}/pipeline_schedules" |
             jq "."

	To get an individual pipeline schedule:

		GET /projects/:id/pipeline_schedules/:pipeline_schedule_id

	just add the schedule id to the end of the route.

### List all pipelines for a particular schedule:

	curl --request GET \
	     --header "PRIVATE-TOKEN: $SELF_GITLAB_TOKEN" \
	     "https://gitlab.ellisbs.co.uk/api/v4/projects/6/pipeline_schedules/5/pipelines" |
	     jq "."

### Create a new pipeline schedule of a project.

	POST /projects/:id/pipeline_schedules

	So to create a random time for a pipeline:

		# Generate random hour (0-23) and minute (0-59)
		RANDOM_HOUR=$((RANDOM % 24))
		RANDOM_MINUTE=$((RANDOM % 60))
		
		# Format the cron expression
		CRON_EXPRESSION="$RANDOM_MINUTE $RANDOM_HOUR * * *"
		
		# Run the curl command to create the scheduled job
		curl --request POST \
		     --header "PRIVATE-TOKEN: $SELF_GITLAB_TOKEN" \
		     --form description="Build packages" \
		     --form ref="main" \
		     --form cron="$CRON_EXPRESSION" \
		     --form cron_timezone="UTC" \
		     --form active="true" \
		     "https://gitlab.ellisbs.co.uk/api/v4/projects/1/pipeline_schedules"


# Users:

	List the users:

		curl --request GET \
	             --header "PRIVATE-TOKEN: $SELF_GITLAB_TOKEN" \
	             "https://gitlab.ellisbs.co.uk/api/v4/users" | 
		jq .

# Groups:

	List the groups:

		curl --request GET \
		     --header "PRIVATE-TOKEN: $SELF_GITLAB_TOKEN" \
	             "https://gitlab.ellisbs.co.uk/api/v4/groups" |
	             jq "."

		The `jq "."` to make it pretty.

		Add "?search=group-name" to find group "group-name".

# Get list of gitlab metrics from prometheus:

	curl -s 'http://192.168.0.124:9090/api/v1/label/__name__/values' | 
	     jq -r '.data[]' |
	     grep '^gitlab_'
