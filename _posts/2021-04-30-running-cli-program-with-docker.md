---
layout: post
title: Running CLI program with Docker image
date: 2021-04-30 16:43:00 +0700
categories: [php]
tags: [Docker, PHP]
---

# Running CLI program with Docker

Recently, I had a chance to work with a WordPress plugin and needed to add support for [i18n](https://developer.wordpress.org/apis/handbook/internationalization/localization/#generate-mo-file).
I didn't have [wp-cli](https://wp-cli.org/) installed and preferred not to install it globally. Since I work at a consulting company, my tech stack changes frequently between Java, Node.js, and PHP.

A coworker showed me that I can run CLI programs directly using Docker images and the [docker run](https://docs.docker.com/engine/reference/commandline/run/) command.

Example:

```
docker run -it --rm -v "$(pwd)":/var/www/html wordpress:cli i18n make-pot . languages/my-plugin.pot --debug
```

In order to make the [make-pot](https://developer.wordpress.org/cli/commands/i18n/make-pot/) command work, navigate to the root folder of `my-plugin`.

## Explanation

- `-it`: Combination of `-i` (interactive) and `-t` (pseudo-TTY).
- `--rm`: Automatically removes the container when it exits.
- `-v "$(pwd)":/var/www/html`: Because the command runs inside an isolated container, we mount our current host directory into the container. When the command completes, the generated output appears directly in our local folder. Without this volume mount, Docker would keep the output inside the container filesystem and we wouldn't receive the files.
- `wordpress:cli`: The Docker image used to run the CLI tools (available on [Docker Hub](https://hub.docker.com/)).
- `make-pot . languages/my-plugin.pot`: The command executed inside the container; the paths `. languages/my-plugin.pot` are evaluated relative to `/var/www/html`.
- `--debug`: Enables debug logging during POT file generation.

## Tips

- When mapping a directory to a container, check the `WORKDIR` of the image. For example, the [wp-cli image](https://github.com/docker-library/wordpress/blob/master/cli/php7.3/alpine/Dockerfile) sets `WORKDIR` to `/var/www/html`. You can also override the working directory with the `-w` flag in `docker run`.
