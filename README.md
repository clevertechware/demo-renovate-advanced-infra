# demo-renovate-advanced-infra

Instance Postgres partagée par `service-factories` et `service-plan`. Dépôt de démo de la série sur Renovate : l'image
`postgres:16.4` et `actions/checkout@v4` sont volontairement anciennes pour que Renovate propose des mises à jour.

La configuration Renovate vient du preset `infra` de `demo-renovate-advanced-config`
(`.github/renovate.json`). Ce dépôt n'a ni workflow Renovate ni secret : le passage est fait par le workflow du dépôt de
configuration.

Le superutilisateur `postgres` / `postgres` est réservé à la démo locale.

## Prérequis

Docker avec Compose v2.

## Lancer

```bash
docker compose up --detach --wait
```

`initdb/01-databases.sql` crée les rôles et bases `factories` (mot de passe `factories`) et `plan` (mot de passe `plan`).
Le script ne s'exécute qu'à l'initialisation du volume.

`PGDATA` est fixé à `/var/lib/postgresql/data`. Les images Postgres 18 et suivantes refusent de démarrer quand un volume est
monté sur ce chemin et que `PGDATA` a sa valeur par défaut (`/var/lib/postgresql/<version>/docker`). Avec cette valeur
explicite, le même `compose.yaml` démarre en 16 comme en 18.

## Tester

```bash
docker compose config --quiet
scripts/check-databases.sh
docker compose down --volumes
```
