---
layout: post
title: Ubuntu Chroot Setup Notes
---

I use `debootstrap` and this helper
[script](https://github.com/clchiou/scripts/blob/master/enter-chroot.sh).

Bootstrap:

    sudo apt-get install debootstrap
    sudo debootstrap trusty ${CHROOT} http://archive.ubuntu.com/ubuntu/

Basic setup inside chroot as root:

    apt-get install locales dialog
    locale-gen en_US.UTF-8

    useradd -m clchiou
    usermod -aG sudo clchiou
    passwd clchiou
