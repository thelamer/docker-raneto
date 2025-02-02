[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)](https://linuxserver.io)

The [LinuxServer.io](https://linuxserver.io) team brings you another container release featuring :-

 * regular and timely application updates
 * easy user mappings (PGID, PUID)
 * custom base image with s6 overlay
 * weekly base OS updates with common layers across the entire LinuxServer.io ecosystem to minimise space usage, down time and bandwidth
 * regular security updates

Find us at:
* [Discord](https://discord.gg/YWrKVTn) - realtime support / chat with the community and the team.
* [IRC](https://irc.linuxserver.io) - on freenode at `#linuxserver.io`. Our primary support channel is Discord.
* [Blog](https://blog.linuxserver.io) - all the things you can do with our containers including How-To guides, opinions and much more!

# [linuxserver/raneto](https://github.com/linuxserver/docker-raneto)
[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/raneto.svg)](https://microbadger.com/images/linuxserver/raneto "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/raneto.svg)](https://microbadger.com/images/linuxserver/raneto "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/raneto.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/raneto.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-raneto/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-raneto/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/raneto/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/raneto/latest/index.html)

[Raneto](http://raneto.com/) - is an open source Knowledgebase platform that uses static Markdown files to power your Knowledgebase.

[![raneto](https://raw.githubusercontent.com/gilbitron/Raneto/master/logo/logo_readme.png)](http://raneto.com/)

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/raneto` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |
| arm64 | arm64v8-latest |
| armhf | arm32v7-latest |


## Usage

Here are some example snippets to help you get started creating a container.

### docker

```
docker create \
  --name=raneto \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -p 3000:3000 \
  -v <path to data>:/config \
  --restart unless-stopped \
  linuxserver/raneto
```


### docker-compose

Compatible with docker-compose v2 schemas.

```
---
version: "2"
services:
  raneto:
    image: linuxserver/raneto
    container_name: raneto
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    volumes:
      - <path to data>:/config
    ports:
      - 3000:3000
    restart: unless-stopped
```

## Parameters

Container images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

| Parameter | Function |
| :----: | --- |
| `-p 3000` | The port for the Raneto web interface |
| `-e PUID=1000` | for UserID - see below for explanation |
| `-e PGID=1000` | for GroupID - see below for explanation |
| `-e TZ=Europe/London` | Specify a timezone to use EG Europe/London |
| `-v /config` | Raneto config and Markdown files |

## User / Group Identifiers

When using volumes (`-v` flags) permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1000` and `PGID=1000`, to find yours use `id user` as below:

```
  $ id username
    uid=1000(dockeruser) gid=1000(dockergroup) groups=1000(dockergroup)
```


&nbsp;
## Application Setup

Access the webui at http://<your-ip>:3000

The default username and password is *admin/password*

This application can only be configured through file storage the web interface is only for editing Markdown files.
You need to understand the following paths and the role they play for the application: 

* /config/config.default.js - Main configuation file to setup your user/site name/etc
* /config/content - All of your markdown files go here [more info](http://docs.raneto.com/usage/creating-pages)
* /config/images - This folder will serve content on http://<your-ip>:3000/images/<image name>.png you can put anything in here but it is specifically for image files so you can embedd them in your markdown files without using external hosting.



## Support Info

* Shell access whilst the container is running: `docker exec -it raneto /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f raneto`
* container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' raneto`
* image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/raneto`

## Updating Info

Most of our images are static, versioned, and require an image update and container recreation to update the app inside. With some exceptions (ie. nextcloud, plex), we do not recommend or support updating apps inside the container. Please consult the [Application Setup](#application-setup) section above to see if it is recommended for the image.  
  
Below are the instructions for updating containers:  
  
### Via Docker Run/Create
* Update the image: `docker pull linuxserver/raneto`
* Stop the running container: `docker stop raneto`
* Delete the container: `docker rm raneto`
* Recreate a new container with the same docker create parameters as instructed above (if mapped correctly to a host folder, your `/config` folder and settings will be preserved)
* Start the new container: `docker start raneto`
* You can also remove the old dangling images: `docker image prune`

### Via Docker Compose
* Update all images: `docker-compose pull`
  * or update a single image: `docker-compose pull raneto`
* Let compose update all containers as necessary: `docker-compose up -d`
  * or update a single container: `docker-compose up -d raneto`
* You can also remove the old dangling images: `docker image prune`

### Via Watchtower auto-updater (especially useful if you don't remember the original parameters)
* Pull the latest image at its tag and replace it with the same env variables in one run:
  ```
  docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --run-once raneto
  ```

**Note:** We do not endorse the use of Watchtower as a solution to automated updates of existing Docker containers. In fact we generally discourage automated updates. However, this is a useful tool for one-time manual updates of containers where you have forgotten the original parameters. In the long term, we highly recommend using Docker Compose.

* You can also remove the old dangling images: `docker image prune`

## Building locally

If you want to make local modifications to these images for development purposes or just to customize the logic: 
```
git clone https://github.com/linuxserver/docker-raneto.git
cd docker-raneto
docker build \
  --no-cache \
  --pull \
  -t linuxserver/raneto:latest .
```

The ARM variants can be built on x86_64 hardware using `multiarch/qemu-user-static`
```
docker run --rm --privileged multiarch/qemu-user-static:register --reset
```

Once registered you can define the dockerfile to use with `-f Dockerfile.aarch64`.

## Versions

* **28.06.19:** - Rebasing to alpine 3.10.
* **01.06.19:** - Initial Release.
