# Based on:
# https://github.com/microsoft/vscode-dev-containers/blob/master/containers/cpp/.devcontainer/Dockerfile
FROM debian:10

# Non-root user data
ARG USERNAME=dev
# If Linux Host, set your UID/GID here
# https://aka.ms/vscode-remote/containers/non-root-user
ARG USER_UID=1000
ARG USER_GID=$USER_UID

ENV DEBIAN_FRONTEND=noninteractive

RUN \
    # Configure apt and install packages
    apt-get update && apt-get -y install apt-utils dialog 2>&1 \
    && apt-get -y install \
    # Essentials
    sudo \
    git \
    iproute2 \
    procps \
    gnupg \
    lsb-release \
    man-db \
    locales \
    # C/C++ Tools
    build-essential \
    cmake \
    cppcheck \
    valgrind \
    # [optional] Additional tools
    bash-completion \
    software-properties-common \
    curl \
    wget \
    unzip \
    nano \
    emacs-nox \
    # Apt Clean up
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/* \
    # [optional] Enable apt install autocomplete
    # https://askubuntu.com/a/1026978
    && sed -i -e 's/^/# /' /etc/apt/apt.conf.d/docker-clean \
    # Create a non-root user
    # https://aka.ms/vscode-remote/containers/non-root-user
    && groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    # Hotfix of an issue with a dependency of the C/C++ Extension
    # https://github.com/microsoft/vscode-cpptools/issues/3174#issuecomment-492426034
    && link /lib/x86_64-linux-gnu/libtinfo.so.6 /lib/x86_64-linux-gnu/libtinfo.so.5 \
    # Configure locales
    && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && dpkg-reconfigure --frontend=noninteractive locales \
    && update-locale LANG=en_US.UTF-8
ENV LANG=en_US.UTF-8

# [optional] Install oh-my-bash
USER dev
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmybash/oh-my-bash/master/tools/install.sh)"
ENV DEBIAN_FRONTEND=
USER root