Install velero:

	wget https://github.com/vmware-tanzu/velero/releases/download/v1.12.0/velero-v1.12.0-linux-amd64.tar.gz
	tar -xvf velero-v1.12.0-linux-amd64.tar.gz
	sudo mv velero-v1.12.0-linux-amd64/velero /usr/local/bin/

Setup installation of velero to back up to aws:

	velero install   --provider aws \
                         --plugins velero/velero-plugin-for-aws:v1.7.0 \
                         --bucket backups \
                         --backup-location-config region=eu-west-2,s3Url=http://192.168.0.14:4566,s3ForcePathStyle=true \
                         --secret-file ./velero-credentials \
                         --use-volume-snapshots=false

	This was with a fake aws, localstack, for which I created creds:

	cat << EOF > velero-credentials
	[default]
	aws_access_key_id = test
	aws_secret_access_key = test
	EOF

Do something like `kubectl get all -n velero` to check on the status of the velero namespace.

Create backup like this:

	velero backup create $(date +"%Y-%m-%d-%H-%M-%S").backup --include-cluster-resources=true

Check status of backups:

	velero get backups

The status of backups are:

	New: 			The backup has been created but hasn't started processing yet.
	InProgress: 		The backup is currently running.
	Completed: 		The backup has finished successfully.
	PartiallyFailed: 	The backup completed, but there were some errors.
	Failed: 		The backup encountered errors and did not complete successfully.

To delete a backup:

	velero delete backup 2024-12-17-14-13-23.backup

To get what could be restore:

	velero get restores

To restore a backup:

	velero restore create --from-backup 2024-12-17-15-48-03.backup

To check progress of restore:

	velero restore describe 2024-12-17-15-48-03.backup-20241217155111

or:

	velero restore logs 2024-12-17-15-48-03.backup-20241217155111
