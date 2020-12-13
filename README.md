# Development environment

Contains some config to make vscode a client for remote development using docker containers.

Requires docker accessible directly, or via ssh. Also requires gnupg running on the same machine where docker runs.

Provides in the container the docker client which will connect to the same docker engine which runs the container.
Also provides docker-compose, gnupg (inherited from the remote machine), rust and some other software.

## Manual actions needed

On the remote machine add line 
```
extra-socket /home/<your username>/.gnupg/S.gpg-agent.vscode
```
in `~/.gnupg/gpg-agent.conf`. It makes gnupg to create dedicated socket.

On the local machine in `devcontainer.json` replace the path `/home/vladislav/.gnupg/S.gpg-agent.vscode` with the actual path of gnupg dedicated socket.

Place your `.gitconfig` into `~/workspace` directory in the container.

## Open problems

* It looks like the vscode extensions are not persist, `vscode-lldb` and `rust-analyzer` downloading their binaries every time the container restart. 
* git (gpg) does not ask password to sign commit. It is bad for security.
* Not matter use this config in vscode, or do not use, giving ssh access to gnupg socket means giving ability to sign messages on behalf of you. Protect your user by strong password if open ssh access world wide.
