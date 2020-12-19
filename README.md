# Development environment

Contains some config to make vscode a client for remote development using docker containers.

Requires docker accessible directly, or via ssh.

Provides in the container the docker client which will connect to the same docker engine which runs the container.
Also provides docker-compose, gnupg, rust and some other software.

## Use on MacOS

Install pinentry for git signing. Install docker client.

`brew install pinentry-mac docker`

## Manual actions needed

Place your `.gitconfig` into `~/workspace` directory in the container.

Remove `--privileged` from `runArgs` if you don't need it.

## Open problems

* It looks like the vscode extensions are not persist, `vscode-lldb` and `rust-analyzer` downloading their binaries every time the container restart. 
