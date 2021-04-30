---
layout: post
title: Running CLI program with Docker image
date:   2021-04-30 16:43:00 +0700
categories: [Docker, PHP]
---

# Running CLI program with Docker

Recently, I have a change to work with Wordpress Plugin and I have to make it support [i18n](https://developer.wordpress.org/apis/handbook/internationalization/localization/#generate-mo-file).
I do not have [wp-cli](https://wp-cli.org/) and i do not want to install it, as I am working for a consultant company, My stack has to change quite often from Java to NodeJs, and PHP.

My coworker showed me that I can run CLI program with an Docker image and [docker run](https://docs.docker.com/engine/reference/commandline/run/) command.

Example:

```
docker run -it --rm -v "$(pwd)":/var/www/html wordpress:cli make-pot . languages/my-plugin.pot
```

In order to make the [make-pot](https://developer.wordpress.org/cli/commands/i18n/make-pot/) command work, we have to change location to the root folder of `my-plugin`

## Explanation

- `-it`: Combination of `-i` and `-t`.
- `--rm`: Automatically remove the container when it exits.
- `-v "$(pwd)":/var/www/html`: On docker run, it runs inside an isolated container from our host, so we need to mount the current folder into the docker container, when the command completed, we will have result in our current directory. If we don't mount, docker will run the command and keep the result in side a auto generated container and we won't received anything.
- `wordpress:cli` : is the docker image, we can search for a lot of images in [DockerHub](https://hub.docker.com/).
- `make-pot . languages/my-plugin.pot`: it's the command will run inside container, the paths `. languages/my-plugin.pot` are inside container.

## Tips

When mapping a directory to a container, you should check the `WORKDIR` of the image. E.g: the [wp-cli image](https://github.com/docker-library/wordpress/blob/master/cli/php7.3/alpine/Dockerfile) has `WORKDIR` at `/var/www/html` , also you can change the `workdir` in docker run command with `-w`
