# List repositories in registry:

	curl  https://docker.ellisbs.co.uk:5190/v2/_catalog|jq .

# List tags in repository:

	curl https://docker.ellisbs.co.uk:5190/v2/systemd/tags/list | jq .

	Where the repository name contains characters like `/` one needs to use usr encoding. eg for `gitlab/gitlab-ce`:

		curl https://docker.ellisbs.co.uk:5190/v2/gitlab%2Fgitlab-ce/tags/list | jq .

# Podman

podman machine init

Machine init complete

To start your machine run:

	podman machine start

Starting machine "podman-machine-default"

This machine is currently configured in rootless mode. If your containers
require root permissions (e.g. ports < 1024), or if you run into compatibility
issues with non-podman clients, you can switch using the following command:

	podman machine set --rootful

API forwarding listening on: /var/folders/rw/__l8ysvj4q9_8z94y4m87crh0000gn/T/podman/podman-machine-default-api.sock

The system helper service is not installed; the default Docker API socket
address can't be used by podman. If you would like to install it, run the following commands:

        sudo /opt/homebrew/Cellar/podman/5.3.1_1/bin/podman-mac-helper install
        podman machine stop; podman machine start

You can still connect Docker API clients by setting DOCKER_HOST using the
following command in your terminal session:

        export DOCKER_HOST='unix:///var/folders/rw/__l8ysvj4q9_8z94y4m87crh0000gn/T/podman/podman-machine-default-api.sock'

Machine "podman-machine-default" started successfully

Obviously to stop your machine run:

	podman machine stop
