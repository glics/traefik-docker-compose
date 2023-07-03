# Reverse Proxy Docker Compose services with Traefik

This repo contains the configuration files for Traefik, used in production as reverse proxy to host multiple Wordpress sites on the same server.

The advantages of this approach are:

- It's easy to manage multiple sites and add new ones
- SSL certificates are automatically generated and renewed by Traefik using Let's Encrypt
- In case it's needed, it's easy to move a site to another server
  - just copy the site folder, install docker, start the Compose stack and update the DNS records

All that's required is installing Docker and Docker Compose (both can be easily installed using the [convenience install script](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script)) and to create a docker network in bridge mode, named `traefik-net`:

```bash
docker network create -d bridge traefik-net
```

After that, you can start the Compose stack using `docker-compose up -d` in the directory containing the `docker-compose.yml` file.

Other services are supposed to provide their own docker compose stack, they should not publish any port, just connect to the `traefik-net` network and add a .yml file for them in the `dynamic-config` folder.

Explaination for the provided configuration files can be found as comments in the files themselves.

Further documentation for Traefik can be found at [docs.traefik.io](https://docs.traefik.io).

NOTE: This configuration is intended for using Let's Encrypt TLS certificates. If you want to use other TLS certificates,
for example from Cloudflare, you need to change the configuration accordingly; see <https://doc.traefik.io/traefik/https/tls/> for more information.
