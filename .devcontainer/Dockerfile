FROM debian:latest

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=${USER_UID}

USER root

RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get update

RUN export DEBIAN_FRONTEND=noninteractive \
    && groupadd --gid ${USER_GID} $USERNAME \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
    && apt-get install -y --no-install-recommends sudo \
    && echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

USER ${USERNAME}

WORKDIR /home/${USERNAME}

RUN export DEBIAN_FRONTEND=noninteractive \
    && sudo apt-get install -y --no-install-recommends \
    bash-completion \
    ca-certificates \
    git \
    python3 \
    python3-pip \
    ssh \
    unzip \
    vim \
    wget \
    # AWS CLI https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
    && wget --quiet "https://awscli.amazonaws.com/awscli-exe-linux-$(uname -m).zip" --output-document="awscliv2.zip" \
    && unzip -u "awscliv2.zip" \
    && rm -rf "awscliv2.zip" \
    && sudo ./aws/install \
    && rm -rf aws \
    && aws --version \
    && echo "complete -C '/usr/local/bin/aws_completer' aws" >> "${HOME}/.bashrc" \
    # AWS SAM CLI https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html
    && wget --quiet "https://github.com/aws/aws-sam-cli/releases/latest/download/aws-sam-cli-linux-x86_64.zip" \
    && mkdir "aws-sam-cli-linux-x86_64" \
    && unzip -u "aws-sam-cli-linux-x86_64.zip" -d "aws-sam-cli-linux-x86_64" \
    && rm -rf "aws-sam-cli-linux-x86_64.zip" \
    && sudo ./aws-sam-cli-linux-x86_64/install \
    && rm -rf "aws-sam-cli-linux-x86_64" \
    && sam --version

# https://docs.docker.com/engine/install/debian/
RUN export DEBIAN_FRONTEND=noninteractive \
    && sudo apt-get update \
    && sudo apt-get install ca-certificates curl \
    && sudo install -m 0755 -d /etc/apt/keyrings \
    && sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc \
    && sudo chmod a+r /etc/apt/keyrings/docker.asc \
    && echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null \
    && sudo apt-get update \
    && sudo apt-get install -y --no-install-recommends \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin \
    && sudo usermod -aG docker ${USERNAME}

RUN export DEBIAN_FRONTEND=noninteractive \
    && sudo apt-get clean \
    && sudo rm -rf /var/lib/apt/lists/*
