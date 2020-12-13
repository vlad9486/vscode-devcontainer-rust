# Development environment

Contains some config to make vscode a client for remote development using docker containers.

The username is 'vladislav' it is to bind some files from remote home directory and retain the same access rights. I do not know if it necessary. Required further investigation. It will be great to find a way how to easily configure the name. Now it is possible to just replace it in `devcontainer.json`.

Requires docker accessible directly, or via ssh. Also requires gnupg running on the same machine where docker runs.

Place your `.gitconfig` into `~/workspace` directory in the container.

Provides in the container the docker client which will connect to the same docker engine which runs the container, docker-compose, gnupg (inherited from the remote), rust and some other software.
