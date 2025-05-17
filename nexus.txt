Get list of privileges:

curl -X 'GET' \
  -u admin \
  "https://${NEXUS_HOST}/service/rest/v1/security/privileges" \
  -H 'accept: application/json' \
  -H 'NX-ANTI-CSRF-TOKEN: 0.763232663911438' \
  -H 'X-Nexus-UI: true'

This will list out all privilege type:

curl -X 'GET' \
  -u admin \
  "https://${NEXUS_HOST}/service/rest/v1/security/privileges" \
  -H 'accept: application/json' \
  -H 'NX-ANTI-CSRF-TOKEN: 0.763232663911438' \
  -H 'X-Nexus-UI: true'| jq '.[].type'|sort -u

On a base install they are:

	"application"
	"repository-admin"
	"repository-view"
	"script"
	"wildcard"

Nonprod has "repository-content-selector" created as well.

There are URLs for

	/rest/v1/security/privileges/application	POST

		curl -X 'POST' \
		  -u admin \
		  "https://${NEXUS_HOST}/service/rest/v1/security/privileges/application" \
		  -H 'accept: application/json' \
		  -H 'Content-Type: application/json' \
		  -H 'NX-ANTI-CSRF-TOKEN: 0.763232663911438' \
		  -H 'X-Nexus-UI: true' \
		  -d '{
		        "type" : "application",
		        "name" : "test-priv",
		        "description" : "Test permissions for Analytics",
		        "readOnly" : false,
		        "actions" : [ "ALL" ],
		        "domain" : "analytics"
		      }'

		Create a new privilege `test-priv`.
		When tested through the API page of console shows strange `Server response`
		```
			Undocumented
			Error: Created
		```

	/rest/v1/security/privileges/repository-admin/{privilegeName}	PUT
	/rest/v1/security/privileges/repository-view/{privilegeName}	PUT
	/rest/v1/security/privileges/repository-content-selector/{privilegeName}	PUT
	/rest/v1/security/privileges/script/{privilegeName}	PUT to update
	/rest/v1/security/privileges/script	POST to create
	/rest/v1/security/privileges/wildcard	PUT to update
		curl -X 'PUT' \
		  -u admin \
		  "https://${NEXUS_HOST}/service/rest/v1/security/privileges/application/nx-analytics-all" \
		  -H 'accept: application/json' \
		  -H 'Content-Type: application/json' \
		  -H 'NX-ANTI-CSRF-TOKEN: 0.763232663911438' \
		  -H 'X-Nexus-UI: true' \
		  -d '{
		  "type" : "application",
		  "name" : "nx-analytics-all",
		  "description" : "All permissions for Analytics",
		  "readOnly" : true,
		  "actions" : [ "ALL" ],
		  "domain" : "analytics"
		}'

		Causes error:
			{
			  "id": "*",
			  "message": "\"Privilege 'nx-analytics-all' is internal and cannot be modified or deleted.\""
			}

		But changing `test-priv` we created earlier should work:

		curl -X 'PUT' \                
                  -u admin \
                  "https://${NEXUS_HOST}/service/rest/v1/security/privileges/application/test-priv" \
                  -H 'accept: application/json' \
                  -H 'Content-Type: application/json' \
                  -H 'NX-ANTI-CSRF-TOKEN: 0.763232663911438' \
                  -H 'X-Nexus-UI: true' \
                  -d '{
                  "type" : "application",
                  "name" : "test-priv",
                  "description" : "All permissions for Analytics",
                  "readOnly" : true,
                  "actions" : [ "READ" ],
                  "domain" : "analytics"
                }'

	/rest/v1/security/privileges/wildcard	POST to create

	This will delete a privilege `test-priv`:

		curl -fX 'DELETE' -u admin \
		     "https://${NEXUS_HOST}/service/rest/v1/security/privileges/test-priv" \ 
		     -H 'accept: application/json' \
		     -H 'NX-ANTI-CSRF-TOKEN: 0.763232663911438'

If one has a file `nexus.priv.json` taken from `nexus.nonprod.dwpcloud.uk` and a file `nexus.priv.fresh.json` taken from a fresh instance, one could do this to get the difference:

	jq --argfile fresh nexus.priv.fresh.json '. - $fresh' nexus.priv.json

Deleting database & re-creating
===============================

	psql -h 192.168.0.12 -U postgres postgres

	drop database gitlab_db;

	create database gitlab_db owner gitlab;

	grant all privileges on database gitlab_db to gitlab;

	ALTER USER gitlab WITH PASSWORD 'LetMeIn2';

Creating a new user
===================

	Launch gitlab-rails console.

	# Create the new user
	user = User.new(
	  username: 'root', # Adjust the username as necessary
	  email: 'newroot@example.com',
	  name: 'New Root User',
	  password: 'securepassword123',
	  password_confirmation: 'securepassword123',
	  admin: true,
	  confirmed_at: Time.now
	)

	# Manually build and assign the namespace to the user
	user.build_namespace(name: user.username, path: user.username, owner: user)

	# Attempt to save the user, which also saves the associated namespace
	begin
	  user.save!
	  puts 'User and namespace created successfully.'
	rescue ActiveRecord::RecordInvalid => e
	  puts "Error creating user: #{e.message}"
	end

Prometheus Metrics
==================

	curl -vvvu admin https://nexus.ellisbs.co.uk/service/metrics/prometheus (need successful login)
