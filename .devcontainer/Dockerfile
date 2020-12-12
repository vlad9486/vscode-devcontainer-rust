FROM ubuntu:20.04

# config
ARG USERNAME
ARG USER_UID
ARG DOCKER_SOCK_GID
ARG DOCKERVERSION
ARG DOCKERCOMPOSEVERSION
ARG RUSTVERSION

# common deps
ENV DEBIAN_FRONTEND='noninteractive'
RUN apt update && apt install -y git wget curl fish vim gnupg2 sudo lsb-release software-properties-common

# docker
RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-$DOCKERVERSION.tgz && \
    tar xzf docker-$DOCKERVERSION.tgz --strip 1 -C /usr/local/bin docker/docker && \
    rm docker-$DOCKERVERSION.tgz && \
    wget https://github.com/docker/compose/releases/download/$DOCKERCOMPOSEVERSION/docker-compose-Linux-x86_64 && \
    chmod +x docker-compose-Linux-x86_64 && mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose

# user
RUN groupadd --gid $DOCKER_SOCK_GID docker && \
    useradd --uid $USER_UID -g users -G docker -ms /usr/bin/fish $USERNAME && \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME
WORKDIR /home/$USERNAME

RUN mkdir -p \
        /home/$USERNAME/.vscode-server/extensions \
        /home/$USERNAME/.vscode-server-insiders/extensions \
        /home/$USERNAME/workspace \
        /home/$USERNAME/.gnupg \
        /home/$USERNAME/.local/share/fish/ && \
        chmod 700 /home/$USERNAME/.gnupg && \
    echo "[includeIf \"gitdir:/home/$USERNAME/workspace/\"]" >> /home/$USERNAME/.gitconfig && \
    echo "\tpath = /home/$USERNAME/workspace/.gitconfig" >> /home/$USERNAME/.gitconfig

# rust
RUN set -eux && \
    wget "https://static.rust-lang.org/rustup/dist/x86_64-unknown-linux-gnu/rustup-init" && \
    chmod +x rustup-init && \
    ./rustup-init -y --default-toolchain $RUSTVERSION && \
    rm rustup-init && \
    mkdir -p .config/fish && \
    echo "set -gx PATH $PATH /home/$USERNAME/.cargo/bin" >> .config/fish/config.fish

# llvm 11
RUN wget https://apt.llvm.org/llvm.sh && chmod +x llvm.sh && sudo ./llvm.sh 11 && rm llvm.sh
ENV LLVM_SYS_110_PREFIX=/usr/lib/llvm-11

RUN sudo apt install -y \
    # some build and run deps
    libclang-dev make clang pkg-config libsodium-dev zlib1g-dev \
    # kernel build deps
    libarchive-tools flex bison libssl-dev bc libelf-dev
