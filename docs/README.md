# helot

> Lire en [Français](/docs/README.fr.md)

Base development Docker images

## About

> Greek: `Εἵλωτες`, phonetic: `Heílôtes`
>
> The helots were a native population of Laconia and Messenia subjugated by the Spartans. They
> were responsible for farming the land and accomplishing domestic chores so that Spartan men
> could dedicate themselves to military training

It's essential to automate work that does not create value

Continuous integration (CI) and continuous delivery (CD) are the first automated workflows any
engineering team should put in place when beginning a project. In that regard, the best strategy
is to use a common shareable development environment that ensures deterministic code execution
for all engineers and bots. This repository contains Ubuntu-based Docker images that can play
this role. They contain basic tools for general development and CI/CD automation (see the
[Dockerfile](/.kano/Dockerfile) for more details)

The default image is based on `Ubuntu 22.04`. Images based on other Ubuntu versions are
suffixed with the version number. All images support AMD64 and AMR64 architectures. These are
the currently supported versions:

- `ghcr.io/logisparte/helot`
- `ghcr.io/logisparte/helot-22.04`
- `ghcr.io/logisparte/helot-20.04`

## License

`helot` is distributed under the terms of the [Apache 2.0 license](/LICENSE)

## Users

### Installation

To use locally:

```shell
docker pull ghcr.io/logisparte/helot
```

To use in a Dockerfile:

```docker
FROM ghcr.io/logisparte/helot

...
```

### Configuration

Some utilities must be configured before their first use in a container

#### GitHub

To configure local git and GitHub credentials :

```shell
kano dockered configure_github "$NAME" "$EMAIL" "$PERSONAL_ACCESS_TOKEN"
```

#### AWS

To configure AWS credentials, simply mount a user `.aws` directory or run the container with
these environment variables:

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_DEFAULT_REGION
- AWS_DEFAULT_OUTPUT

> See AWS CLI's
> [documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) for
> more information

### Defaults

These values are configured by default in the image:

- User: `docker` (passwordless sudo)
- Shell: `/bin/sh -e -c`
- Command: `/bin/sh`
- Timezone: `UTC` (configurable through `--build-arg TIMEZONE`)
- Editor: `vim`

## Contributors

> See [kano](https://github.com/logisparte/kano)'s builtin docker task documentation for more
> information on the following tasks

### Build

To build the development image from Ubuntu 22.04:

```shell
kano docker build
```

To build the development image from another Ubuntu version:

```shell
kano docker build --build-arg UBUNTU_VERSION="20.04"
```

### Test

To test that the image was built correctly:

```shell
kano dockered test
```

### Release

**This should normally be done by the CI/CD pipeline.** To build and release the images to the
registry:

```shell
kano dockered release "$REGISTRY" "$VERSION"
```
