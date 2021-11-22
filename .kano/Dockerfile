ARG UBUNTU_VERSION="20.04"
FROM ubuntu:$UBUNTU_VERSION

LABEL org.opencontainers.image.title="helot-$UBUNTU_VERSION" \
    org.opencontainers.image.description="Base development image" \
    org.opencontainers.image.authors="logisparte <info@logisparte.com>" \
    org.opencontainers.image.source="https://github.com/logisparte/helot"

ARG TIMEZONE="UTC"

ENV EDITOR="/usr/bin/vim"
ENV TIMEZONE="${TIMEZONE}"

CMD /bin/zsh

RUN DEBIAN_FRONTEND=noninteractive \
    ln -snf "/usr/share/zoneinfo/$TIMEZONE" /etc/localtime \
    && echo "$TIMEZONE" > /etc/timezone \
    && apt-get update \
    && apt-get install -y -q --no-install-recommends \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg2 \
        software-properties-common \
    && curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg \
        | dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
        | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
    && apt-get update \
    && apt-get install -y -q --no-install-recommends \
        gh \
        git \
        groff \
        language-pack-fr \
        less \
        ssh-client \
        sudo \
        tzdata \
        unzip \
        vim \
        wget \
        zsh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && chsh -s /bin/zsh root \
    && curl -L https://github.com/logisparte/kano/releases/download/3.3.5/kano.tar.gz \
        | tar -xz \
    && ./kano/install \
    && rm -rf ./kano \
    && curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.3.7.zip -o awscliv2.zip \
    && unzip awscliv2.zip \
    && rm -rf awscliv2.zip \
    && ./aws/install \
    && rm -rf ./aws

COPY sources/kano /etc/kano
