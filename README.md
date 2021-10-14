# docker-circleboom-strapi
Docker compose file for Strapi with Caddy as reverse proxy.

Strapi is the leading open-source headless CMS. It’s 100% JavaScript, fully customizable and developer-first. https://strapi.io

Strapi is an open-source project. The core project, as well as the documentation and any related tool can be found in the Strapi [https://github.com/strapi] GitHub organization.

This docker compose file uses these containers:
- Strapi https://hub.docker.com/r/strapi/strapi
- Caddy v2.2.1 as reverse proxy server https://hub.docker.com/_/caddy

## Environment Variables

To deploy, create an ```.env``` file in the same directory with ```docker-compose.yml``` file at server with your environment variables in it.

```
POSTGRES_SERVER=your-postgres-db-server-ip
POSTGRES_SERVER_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=your-secure-db-password
POSTGRES_TARGET_DB=db-name-you-want-use-with-strapi

CADDY_SITE_ADDRESS=:80
```

## Caddy Config

Caddy config is named ```Caddyfile``` and it is pretty straightforward.

**Default configuration is**
```
{$CADDY_SITE_ADDRESS} {
  reverse_proxy strapi:1337
}
```
which publishes the web interface on the server's port 80 which is defined in ```.env``` file above.

Replace ```:80``` with your domain name to get automatic https via LetsEncrypt. Add your domain to serve only https (recommended)

Don't forget to set your DNS first as Caddy automatically validates and creates certificates for you as soon as it started.

Allow 443/tcp port to outside access before rebuilding your docker-compose. This might be tricky as docker modifies iptables on linux directly https://docs.docker.com/network/iptables/

If you don't want to open your web interface to the public, you could always create a tunnel for it

```
# sudo ssh -N -L 443:127.0.0.1:443 root@server-public-ip -i ~/.ssh/id_rsa
```

and visit https://127.0.0.1 after your docker containers up.

## Some Docker commands

```
// Spin up the containers
# docker-compose up -d

// List active containers
# docker ps

// See the logs
# docker logs your_container_name

// Restart all in the compose file
# docker-compose restart

// Restart an individual container
# docker restart your_container_name
```
