# helot

> Lire en [Français](/docs/README.fr.md)

Base Docker image for process automation

## About

> Greek: `Εἵλωτες`, phonetic: `Heílôtes`
>
> The helots were a native population of Laconia and Messenia subjugated by the Spartans. They
> were responsible for farming the land and accomplishing domestic chores so that Spartan men
> could dedicate themselves to military training

It's essential to automate work that does not create value. This repository contains a
Ubuntu-based Docker image that helps automate processes interacting with _GitHub_ and _Amazon
Web Services_. [kano](https://github.com/logisparte/kano) is also included alongside other
development experience essentials (see the [Dockerfile](/docker/Dockerfile) for more details)

## License

`helot` is _free as in freedom_, under the terms of the [GPL-3.0 License](/LICENSE)

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

#### GitHub

To configure system-wide git and GitHub credentials **from inside the container**:

```shell
kano configure_github "$NAME" "$EMAIL" "$PERSONAL_ACCESS_TOKEN"
```

#### AWS

To configure AWS credentials, simply run the container with these environment variables:

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_DEFAULT_REGION
- AWS_DEFAULT_OUTPUT

> See AWS CLI's
> [documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) for
> more information

## Contributors

### Build

To build the image:

```shell
kano build
```

> Image will be named `helot:local`

### Dev

To run a container in interactive mode:

```shell
kano dev
```

### Test

To test that the image was built correctly **from inside the container**:

```shell
kano test
```
