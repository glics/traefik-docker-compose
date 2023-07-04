# Reverse Proxy Docker Compose stacks with Traefik

This repository includes configuration files for Traefik, which is utilized as a reverse proxy to enable hosting of multiple sites on a single server through the use of multiple Docker Compose stacks.

## Advantages

The reasons for which you may want this Docker Compose/Traefik setup are:

- **Easy management of multiple sites:** With Traefik, it's easy to manage multiple sites and add new ones. You can simply create a new folder for each site, add the necessary configuration files, and Traefik will automatically route traffic to the correct site based on the domain name.
- **Automatic SSL certificate generation and renewal:** Traefik can automatically generate and renew SSL certificates using Let's Encrypt, which means you don't have to worry about manually configuring SSL certificates for each site.
- **Easy site migration:** If you need to move a site to another server, it's easy to do so with Traefik. You can simply copy the site folder, install Docker, start the Compose stacks for Traefik and your site, and update the DNS records to point to the new server.

## Requirements

To use this stack, you need to have Docker and Docker Compose installed on your system. If you don't have them already and you're running a Debian-based distro on your server, you can easily install them using the [convenience install script](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script).

You also need to create a Docker network in bridge mode called `traefik-net`. You can do this by running the following command:

```bash
docker network create -d bridge traefik-net
```

## Usage

To start the Traefik stack, run the following command in the directory containing the `docker-compose.yml` file:

```bash
docker-compose up -d
```

This will start the Traefik reverse proxy and make it available on port 80 (http) and 443 (https).

Other services that you want to expose through Traefik should provide their own Docker Compose stack. They should not publish any ports, but instead connect to the `traefik-net` network and add a `.yml` file for their service in the `dynamic-config` folder.

To connect a service to the `traefik-net` network, add the following to the service's `docker-compose.yml` file:

```yaml
services:
  your-service:
    networks:
      - traefik-net
networks:
  traefik-net:
    external: true
```

## Contributing

If you find any issues with this stack or want to contribute improvements, feel free to open an issue or submit a pull request. We welcome contributions from the community!

Explaination for the provided configuration files can be found as comments in the files themselves.

Further documentation for Traefik can be found at [docs.traefik.io](https://docs.traefik.io).

NOTE: This configuration is intended for using Let's Encrypt TLS certificates. If you want to use other TLS certificates,
for example from Cloudflare, you need to change the configuration accordingly; see <https://doc.traefik.io/traefik/https/tls/> for more information.
