# Sentry server configuration deployment 
### [docker] Sentry server installation

Guide installation
https://docs.getsentry.com/on-premise/server/installation/docker/

* Install redis
`docker run -d --name sentry-redis redis:3.2-alpine`

* Install postgres
`docker run -d --name sentry-postgres --env POSTGRES_PASSWORD=<password> --env POSTGRES_USER=<username> postgres:9.5`

* Install smtp
`docker run -d --name sentry-smtp tianon/exim4`

* Build docker image
`docker build -t <docker_username>/sentry .`

* Getting sentry secret key
`docker run --rm <docker_username>/sentry config generate-secret-key`

* Upgrade for db init
`docker run --rm -it --link sentry-redis:redis --link sentry-postgres:postgres --link sentry-smtp:smtp --env SENTRY_SECRET_KEY="<secret_key_from_5_step>" <docker_username>/sentry upgrade`

* Run web host
`docker run -d --name sentry-web-01 -p 9000:9000 --link sentry-redis:redis --link sentry-postgres:postgres --link sentry-smtp:smtp --env SENTRY_SECRET_KEY="<secret_key_from_5_step>" <docker_username>/sentry run web`

* Run capture worker
`docker run -d --name sentry-worker-01 --link sentry-redis:redis --link sentry-postgres:postgres --link sentry-smtp:smtp --env SENTRY_SECRET_KEY="<secret_key_from_5_step>" <docker_username>/sentry run worker`

* Run cron
`docker run -d --name sentry-cron --link sentry-redis:redis --link sentry-postgres:postgres --link sentry-smtp:smtp --env SENTRY_SECRET_KEY="<secret_key_from_5_step>" <docker_username>/sentry run cron`

Open browser and check url http://<ip_where_container_started>:9000

___
Get docker image
`docker pull isemin/sentry`
