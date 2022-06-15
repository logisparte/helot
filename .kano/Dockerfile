ARG UBUNTU_VERSION="22.04"
FROM ubuntu:$UBUNTU_VERSION

ARG TARGETARCH
ARG TIMEZONE="UTC"

ENV EDITOR="/usr/bin/vim"
ENV TIMEZONE="${TIMEZONE}"

CMD /bin/zsh

COPY .kano/installers ./installers

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
    && chsh -s /bin/zsh root \
    && curl -L "https://github.com/logisparte/kano/releases/download/3.5.3/kano.tar.gz" \
        | tar -xz \
    && ./kano/install \
    && ./installers/aws "$TARGETARCH" \
    # cleanup
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf ./kano \
    && rm -rf ./installers

COPY sources/kano /etc/kano
