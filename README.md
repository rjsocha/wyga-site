# WYGA Site Local Gateway

The WYGA Site service provides a tailored solution for developers' environments. This project aims to simplify the exposure of multiple docker's services, ensuring each is equipped with valid TLS certificates.

All DNS requests to ***.wyga.site** resolve to the **127.0.0.1** IP address.

This project is powered by [Traefik](https://traefik.io/traefik/).

Check out [wyga-site-trafik](https://github.com/rjsocha/wyga-site-traefik) for gateway image.

## Installation

### Setup

  ```
  docker network inspect gateway || docker network create gateway
  ```

### Deployment

To deploy, clone this repository to your local machine by executing the following commands:

  ```bash
  mkdir -p ~/.wyga-site && cd ~/.wyga-site
  curl -sO https://raw.githubusercontent.com/rjsocha/wyga-site/main/compose.yaml
  docker compose up -d --pull always
  ```

Please keep in mind that you need to refresh this image periodically by pulling a newer version.\
**Certificates are generated every two weeks and remain valid for approximately 90 days (+- 2 weeks).**

**You can rerun the above command at least once every 3 months or when the embedded certificates have expired.**

You can locate the service folder by running the following command:

```
docker compose ls | grep wyga-site
```

## Usage

### Dashboard

  Traefik's dashboard is enabled by default and can be accessed at this URL: https://traefik.wyga.site/

### Certificates

  This service offers the following TLS certificates:

  ```
   *.wyga.site
  ```

### Exposing Services (docker compose)

  Define **wyga.expose** label and add **gateway** network reference:

  ```
  ---
  version: "3"
  name: "example"

  services:
    nginx:
      image: nginx:stable-alpine
      labels:
        - wyga.expose=example.wyga.site
      networks:
        gateway:
          priority: 100
        default:
          priority: 500

  networks:
    gateway:
      external: true
  ```

  You can publish a service with multiple names (separate them with commas). If the hostname ends with an '\*', the wildcard name will be published in the format 'name-\*.wyga.site'. For example:

```
  - wyga.expose=example*.wyga.site
```

### From CLI

  Examples:

  ```
  docker run --label wyga.expose=apache.wyga.site --network gateway --rm --name apache httpd:alpine
  ```

  ```
  docker run --label wyga.expose=pma.wyga.site --network gateway --rm --name phpmyadmin phpmyadmin
  ```
