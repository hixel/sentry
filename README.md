# Sentry server configuration deployment 
[docker] Sentry server installation

Guide installation
https://docs.getsentry.com/on-premise/server/installation/docker/

Install redis
1. docker run -d --name sentry-redis redis:3.2-alpine

Install postgres
2. docker run -d --name sentry-postgres --env POSTGRES_PASSWORD=<password> --env POSTGRES_USER=<username> postgres:9.5

Install smtp
3. docker run -d --name sentry-smtp tianon/exim4

Build docker image
4. docker build -t <docker_username>/sentry .

Getting sentry secret key
5. docker run --rm <docker_username>/sentry config generate-secret-key

Upgrade for db init
6. docker run --rm -it --link sentry-redis:redis --link sentry-postgres:postgres --link sentry-smtp:smtp --env SENTRY_SECRET_KEY="<secret_key_from_5_step>" <docker_username>/sentry upgrade

And run
7. docker run -d --name sentry-web-01 -p 9000:9000 --link sentry-redis:redis --link sentry-postgres:postgres --link sentry-smtp:smtp --env SENTRY_SECRET_KEY="<secret_key_from_5_step>" <docker_username>/sentry run web


Open browser and check url http://<ip_where_container_started>:9000
