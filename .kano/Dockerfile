# syntax=docker/dockerfile:1
ARG UBUNTU_VERSION="22.04"
FROM ubuntu:$UBUNTU_VERSION AS base

ARG TIMEZONE="UTC"

RUN <<eof
  ln -snf "/usr/share/zoneinfo/$TIMEZONE" /etc/localtime
  echo "$TIMEZONE" > /etc/timezone
  apt-get update
  DEBIAN_FRONTEND=noninteractive apt-get install --yes --quiet --no-install-recommends \
    apt-transport-https \
    ca-certificates \
    curl \
    git \
    gnupg2 \
    software-properties-common \
    ssh-client \
    sudo \
    tzdata \
    unzip \
    wget
eof

FROM base AS zsh

RUN <<eof
  DEBIAN_FRONTEND=noninteractive apt-get install --yes --quiet --no-install-recommends \
    zsh
  chsh -s /bin/zsh root
eof

CMD /bin/zsh

FROM zsh AS vim

ENV EDITOR="/usr/bin/vim"

RUN <<eof
  DEBIAN_FRONTEND=noninteractive apt-get install --yes --quiet --no-install-recommends \
    vim
eof

FROM vim AS kano

COPY sources/kano /etc/kano

RUN <<eof
  curl --silent --show-error --location \
    "https://github.com/logisparte/kano/releases/download/3.6.0/kano.tar.gz" \
      | tar --extract --gzip
  ./kano/install
  rm -rf ./kano
eof

FROM kano AS github

RUN <<eof
  curl --silent --show-error --location \
    "https://cli.github.com/packages/githubcli-archive-keyring.gpg" \
      | dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
    | tee /etc/apt/sources.list.d/github-cli.list > /dev/null
  apt-get update
  DEBIAN_FRONTEND=noninteractive apt-get install --yes --quiet --no-install-recommends \
    gh
eof

FROM github as aws

ARG TARGETARCH

RUN <<eof
  DEBIAN_FRONTEND=noninteractive apt-get install --yes --quiet --no-install-recommends \
    groff \
    less

  case "$TARGETARCH" in
    amd64)
      AWS_ARCHITECTURE="x86_64"
      ;;

    arm64*)
      AWS_ARCHITECTURE="aarch64"
      ;;

    *)
      echo "Unknown architecture $TARGETARCH" >&2
      exit 1
      ;;
  esac

  curl --silent --show-error --location --output "aws.zip" \
    "https://awscli.amazonaws.com/awscli-exe-linux-$AWS_ARCHITECTURE-2.7.7.zip"
  unzip aws.zip
  rm -rf aws.zip
  ./aws/install
  rm -rf ./aws
eof

FROM aws as cleanup

RUN <<eof
  apt-get clean
  rm -rf /var/lib/apt/lists/*
eof
