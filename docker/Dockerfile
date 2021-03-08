FROM ubuntu:20.04

LABEL name="helot" \
    url="https://github.com/logisparte/helot" \
    author="logisparte" \
    author_email="info@logisparte.com" \
    description="Base Docker image for process automation"

RUN DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get install -y -q --no-install-recommends \
    gnupg \
    software-properties-common \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-key C99B11DEB97541F0 \
    && apt-add-repository https://cli.github.com/packages \
    && apt-get install -y -q --no-install-recommends \
    ca-certificates \
    curl \
    gh \
    git \
    groff \
    less \
    unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && curl -L https://github.com/logisparte/kano/releases/latest/download/kano.tar.gz | tar -xz \
    && ./kano/install \
    && rm -rf ./kano \
    && curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscliv2.zip \
    && unzip awscliv2.zip \
    && rm -rf awscliv2.zip \
    && ./aws/install \
    && rm -rf ./aws

COPY .kano_global /root/.kano_global

# GitHub actions run the container with HOME=/github/home
RUN mkdir -p /github/home \
    && ln -s /root/.kano_global /github/home/.kano_global
