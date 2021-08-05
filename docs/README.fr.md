# helot

> Read in [English](/docs/README.md)

Image Docker de base pour automatisation de processus

## À propos

> Grec: `Εἵλωτες`, phonétique: `Heílôtes`
>
> Les hilotes étaient une population autochtone de Laconie et de Messénie asservie aux
> Spartiates. Ils étaient responsables de cultiver les terres et d'accomplir les tâches
> domestiques afin que les spartiates puissent dédier leur temps à l'entrainement militaire

Il est essentiel d'automatiser le plus possible le travail qui ne crée pas de valeur. Ce dépôt
contient une image Docker basée sur Ubuntu aidant à automatiser des processus interagissant avec
_GitHub_ et _Amazon Web Services_. [kano](https://github.com/logisparte/kano) est aussi inclus
ainsi que d'autres dépendances incontournables pour l'expérience de développement (voir le
[Dockerfile](/docker/Dockerfile) pour de plus de détails)

## Licence

`helot` est _libre comme dans liberté_, sous les termes de la [licence GPL-3.0](/LICENSE)

## Utilisateurs

### Installation

Pour l'utiliser directement en local :

```shell
docker pull ghcr.io/logisparte/helot
```

Pour l'utiliser dans un Dockerfile :

```docker
FROM ghcr.io/logisparte/helot

...
```

### Configuration

#### GitHub

Pour configurer les identifiants git et GitHub locaux **depuis l'intérieur du conteneur** :

```shell
kano configure_github "$NAME" "$EMAIL" "$PERSONAL_ACCESS_TOKEN"
```

#### AWS

Pour configurer les identifiants AWS, simplement exécuter le conteneur avec ces variables
d'environnement :

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_DEFAULT_REGION
- AWS_DEFAULT_OUTPUT

> Voir la
> [documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)
> d'AWS CLI pour plus d'information

## Contributeurs

### Build

Pour bâtir l'image :

```shell
kano build
```

> L'image sera nommée `helot:local`

### Dev

Pour exécuter un conteneur en mode interactif :

```shell
kano dev
```

### Test

Pour tester que l'image a bien été bâtie **depuis l'intérieur du conteneur** :

```shell
kano test
```
