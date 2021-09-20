# helot

> Read in [English](/docs/README.md)

Image Docker de développement de base

## À propos

> Grec: `Εἵλωτες`, phonétique: `Heílôtes`
>
> Les hilotes étaient une population autochtone de Laconie et de Messénie asservie aux
> Spartiates. Ils étaient responsables de cultiver les terres et d'accomplir les tâches
> domestiques afin que les spartiates puissent dédier leur temps à l'entrainement militaire

Il est essentiel d'automatiser le travail qui ne crée pas de valeur

L'intégration continue (CI) and le déploiement continu (CD) sont les premiers processus
automatisés qu'une équipe d'ingénierie devrait mettre en place au démarrage d'un projet. À cette
fin, la meilleure stratégie est d'utiliser un environnement de développement commun et
partageable qui assure une exécution déterministique du code pour tous les ingénieurs et
automates. Ce dépôt contient des images Docker basées sur Ubuntu pouvant jouer ce rôle. Elles
contiennent des outils de base pour le développement général and l'automatisation CI/CD (voir le
[Dockerfile](/.kano/Dockerfile) pour de plus de détails)

L'image par défaut est basée sur Ubuntu 20.04. Les versions spécifiques sont dénotées par un
suffixe précédé d'un trait d'union. Les versions actuellement supportées sont les suivantes :

- `ghcr.io/logisparte/helot`
- `ghcr.io/logisparte/helot-20.04`
- `ghcr.io/logisparte/helot-18.04`
- `ghcr.io/logisparte/helot-16.04`

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

> Voir la documentation de la tâche _docker_ incluse dans
> [kano](https://github.com/logisparte/kano) pour plus d'informations sur les tâches suivantes

### Build

Pour bâtir l'image à partir d'Ubuntu 20.04 :

```shell
kano docker build
```

Pour bâtir l'image à partir d'une autre version d'Ubuntu :

```shell
kano docker build --build-arg UBUNTU_VERSION="16.04"
```

### Dev

Pour exécuter un conteneur de développement en mode interactif :

```shell
kano docker start
kano docker attach
```

### Test

Pour tester que l'image a bien été bâtie :

```shell
kano docker run kano test

# ou depuis l'intérieur du conteneur (lorsqu'attaché):

kano test
```

### Release

**Ceci devrait normalement être fait par le processus de CI/CD.** Pour bâtir et publier les
images dans le registre :

```shell
kano release "$REGISTRY" "$REPOSITORY" "$VERSION"
```
