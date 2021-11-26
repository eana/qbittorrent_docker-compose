qbittorrent docker-compose
==========================

This micro-repo is used only as a host for an easily hostable [qbittorrent]. We
don't want qbittorrent to be accessible on all interfaces on the server, hence
is limited to the `docker0` interface. On the host machine qbittorrent will be
accessible on `172.17.0.1:8110`.

To access qbittorrent from outside you should have a reverse proxy in front of
it.

Besides qbittorrent a [proftpd] will be started to allow the user to download
its torrents locally.

Application Setup
-----------------

The default username/password is `admin/adminadmin`.
Change username/password via the webui in the webui section of settings.

How to run this
---------------

Clone this repository and create a new `docker-compose.secrets.yml` with the
following content:

```yaml
---
version: "3.7"
services:
  proftpd:
    environment:
      - USERNAME=myusername
      - PASSWORD=mypassword
    volumes:
      - /path/to/downloads:/ftp
  qbittorrent:
    volumes:
      - /path/to/config:/config
      - /path/to/downloads:/downloads
```

---
**NOTE**

It is very important that the download volume defined for the `proftpd` to
match the download volume defined for `qbittorrent`.

---

```bash
docker-compose -f docker-compose.yml -f docker-compose.secrets.yml up -d
```

Last thoughts
-------------

The reason for creating the `docker-compose.secrets.yml` was to prevent
committing secrets to this repository. However, if you don't need to commit
these credentials, you could just combine the `docker-compose.yml` and
`docker-compose.secrets.yml` into one file.

[qbittorrent]:https://www.qbittorrent.org
[proftpd]:https://github.com/eana/docker-proftpd
