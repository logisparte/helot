# syntax=docker/dockerfile:1
ARG UBUNTU_VERSION="22.04"
FROM ubuntu:$UBUNTU_VERSION AS base

RUN <<eof
  apt-get update
  DEBIAN_FRONTEND=noninteractive apt-get install --yes --quiet --no-install-recommends \
    sudo
  useradd --groups sudo docker
  passwd --delete docker
eof

USER docker

SHELL ["/bin/sh", "-e", "-c"]

CMD ["/bin/sh"]

FROM base AS tools

ARG TIMEZONE="UTC"

RUN <<eof
  sudo ln -snf "/usr/share/zoneinfo/$TIMEZONE" /etc/localtime
  sudo sh -c "echo $TIMEZONE > /etc/timezone"
  sudo apt-get update
  DEBIAN_FRONTEND=noninteractive sudo apt-get install --yes --quiet --no-install-recommends \
    apt-transport-https \
    ca-certificates \
    curl \
    git \
    gnupg2 \
    software-properties-common \
    ssh-client \
    tzdata \
    unzip \
    vim \
    wget \
    zsh
  sudo git config --system init.defaultbranch master
eof

ENV EDITOR="/usr/bin/vim"

FROM tools AS kano

COPY sources/kano /etc/kano

RUN <<eof
  curl --silent --show-error --location \
    "https://github.com/logisparte/kano/releases/download/3.6.0/kano.tar.gz" \
      | sudo tar --extract --gzip
  sudo ./kano/install
  sudo rm -rf ./kano
eof

FROM kano AS github

RUN <<eof
  curl --silent --show-error --location \
    "https://cli.github.com/packages/githubcli-archive-keyring.gpg" \
      | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
    | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
  sudo apt-get update
  DEBIAN_FRONTEND=noninteractive sudo apt-get install --yes --quiet --no-install-recommends \
    gh
eof

FROM github as aws

ARG TARGETARCH

RUN <<eof
  DEBIAN_FRONTEND=noninteractive sudo apt-get install --yes --quiet --no-install-recommends \
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

  PACKAGE="aws"
  ARCHIVE="$PACKAGE.zip"
  sudo curl --silent --show-error --location --output "$ARCHIVE" \
    "https://awscli.amazonaws.com/awscli-exe-linux-$AWS_ARCHITECTURE-2.7.12.zip"
  sudo unzip "$ARCHIVE"
  sudo rm -rf "$ARCHIVE"
  sudo "./$PACKAGE/install" --bin-dir "/usr/bin" --install-dir "/opt/aws-cli"
  sudo rm -rf "./$PACKAGE"
eof

FROM aws as cleanup

RUN <<eof
  sudo apt-get clean
  sudo rm -rf /var/lib/apt/lists/*
eof
