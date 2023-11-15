# WYGA Site Local Gateway

The WYGA Site service provides a tailored solution for developers' environments. This project aims to simplify the exposure of multiple docker's services, ensuring each is equipped with valid TLS certificates.

All DNS requests to ***.wyga.site** resolve to the **127.0.0.1** IP address.

This project is powered by [Traefik](https://traefik.io/traefik/).

Check out [wyga-site-trafik](https://github.com/rjsocha/wyga-site-traefik) for gateway image.

## Installation

### Setup

  ```
  docker network inspect wyga-site || docker network create wyga-site
  ```

### Deployment

To deploy, clone this repository to your local machine and execute the following command:
traefik.wyga.site
  ```bash
  git clone https://github.com/rjsocha/wyga-site.git
  cd wyga-site
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
   wyga.site
   *.wyga.site
  ```

### Exposing Services

  Define **expose.vhost** label and add **wyga-site** network reference:

  ```
  ---
  version: "3"
  name: "example"

  services:
    nginx:
      image: nginx:stable-alpine
      labels:
        - expose.vhost=example.wyga.site
      networks:
        wyga-site:
          priority: 100
        default:
          priority: 500

  networks:
    wyga-site:
      external: true
  ```
