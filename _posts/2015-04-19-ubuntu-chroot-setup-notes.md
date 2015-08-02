---
layout: post
title: Ubuntu Chroot Setup Notes
---

I use `debootstrap` and this helper
[script](https://github.com/clchiou/scripts/blob/master/run-chroot.sh).

Bootstrap:

    sudo apt-get install debootstrap
    sudo debootstrap trusty ${CHROOT} http://archive.ubuntu.com/ubuntu/

Basic setup inside chroot as root:

    apt-get install locales dialog
    locale-gen en_US.UTF-8

    vi /etc/hosts  # Add a line like "127.0.1.1 your_hostname"

    useradd -m clchiou --shell /bin/bash  # Default is /bin/sh
    usermod -aG sudo clchiou
    passwd clchiou
