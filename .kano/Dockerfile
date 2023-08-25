# syntax=docker/dockerfile:1
ARG UBUNTU_VERSION="22.04"
FROM ubuntu:$UBUNTU_VERSION AS base

RUN <<EOF
  DEBIAN_FRONTEND=noninteractive \
    apt-get update \
      && apt-get install --yes --quiet --no-install-recommends \
        sudo
  useradd --groups sudo --no-create-home docker
  passwd --delete docker
  apt-get clean
  rm -rf /var/lib/apt/lists/*
EOF

USER docker

SHELL ["/bin/sh", "-e", "-c"]

CMD ["/bin/sh"]

FROM base AS tools

ARG TIMEZONE="UTC"

RUN <<EOF
  sudo ln -snf "/usr/share/zoneinfo/$TIMEZONE" /etc/localtime
  sudo sh -c "echo $TIMEZONE > /etc/timezone"
  DEBIAN_FRONTEND=noninteractive \
    sudo apt-get update \
      && sudo apt-get install --yes --quiet --no-install-recommends \
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
  sudo apt-get clean
  sudo rm -rf /var/lib/apt/lists/*
EOF

ENV EDITOR="/usr/bin/vim"

COPY sources/kano /etc/kano

FROM tools AS github

RUN <<EOF
  curl --silent --show-error --location \
    "https://cli.github.com/packages/githubcli-archive-keyring.gpg" \
      | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
  sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
    | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
  DEBIAN_FRONTEND=noninteractive \
    sudo apt-get update \
      && sudo apt-get install --yes --quiet --no-install-recommends \
        gh
  sudo apt-get clean
  sudo rm -rf /var/lib/apt/lists/*
EOF

FROM github as aws

ARG TARGETARCH

RUN <<EOF
  DEBIAN_FRONTEND=noninteractive \
    sudo apt-get update \
      && sudo apt-get install --yes --quiet --no-install-recommends \
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
    "https://awscli.amazonaws.com/awscli-exe-linux-$AWS_ARCHITECTURE-2.13.13.zip"
  sudo unzip "$ARCHIVE"
  sudo rm -rf "$ARCHIVE"
  sudo "./$PACKAGE/install" --bin-dir "/usr/bin" --install-dir "/opt/aws-cli"
  sudo rm -rf "./$PACKAGE"
  sudo apt-get clean
  sudo rm -rf /var/lib/apt/lists/*
EOF
