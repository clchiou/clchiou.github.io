---
layout: post
title: Docker Cheat Sheet
---

Group `docker` commands into categories.

Image

* `build Dockerfile`
* `commit container` (Create a new image from a container's changes.)
* `history image`
* `images`
* `import url|-`
* `inspect container|image`
* `load < file.tar`
* `rmi image...`
* `save image... > file.tar`

Container

* `cp container:path host_path`
* `create image [command [arg...]]` (Create a container and optionally specify a command.  The new container is not started.)
* `diff container`
* `export container`
* `inspect container|image`
* `port container [private_port[/protocol]]`
* `ps` (List containers)
* `rm container...`
* `run image [command [arg...]]` (Create a container and run a command.  This is equivalent to `create` and `start`.)

Container process management

* `kill container...`
* `pause container`
* `restart container...`
* `run image [command [arg...]]` (Create a container and run a command.  This is equivalent to `create` and `start`.)
* `start container...`
* `stop container...`
* `top container [ps options]` (List processes of a container.)
* `unpause container`

Interact with container process

* `attach container` (Interact with the primary process `pid 1`.)
* `exec container command [arg...]` (Run a new command in a running container.)
* `logs container`
* `wait container...`

Docker Hub

* `pull name[:tag]`
* `push name[:tag]`
* `search term`
* `tag image[:tag] name[:tag]`

Misc

* `events`
* `info`
* `login`
* `logout`
* `version`
