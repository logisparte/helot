FROM ubuntu:20.04

LABEL org.opencontainers.image.title="helot" \
    org.opencontainers.image.description="Base Docker image for process automation" \
    org.opencontainers.image.authors="logisparte <info@logisparte.com>" \
    org.opencontainers.image.source="https://github.com/logisparte/helot"

ENV EDITOR="/usr/bin/vim"

CMD /bin/zsh

RUN DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get install -y -q --no-install-recommends \
    gnupg2 \
    software-properties-common \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-key C99B11DEB97541F0 \
    && apt-add-repository https://cli.github.com/packages \
    && apt-get install -y -q --no-install-recommends \
    ca-certificates \
    curl \
    gh \
    git \
    groff \
    language-pack-fr \
    less \
    ssh-client \
    unzip \
    vim \
    zsh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && chsh -s /bin/zsh root \
    && curl -L https://github.com/logisparte/kano/releases/download/3.0.0/kano.tar.gz | tar -xz \
    && ./kano/install \
    && rm -rf ./kano \
    && curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.2.3.zip -o awscliv2.zip \
    && unzip awscliv2.zip \
    && rm -rf awscliv2.zip \
    && ./aws/install \
    && rm -rf ./aws

COPY kano /etc/kano
