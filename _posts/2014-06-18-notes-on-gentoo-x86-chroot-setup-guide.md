---
layout: post
title: Notes on Gentoo x86 Chroot Setup Guide
---

I followed the
[Gentoo x86 Chroot Setup Guide](https://www.gentoo.org/proj/en/base/x86/chroot.xml).
Here are my notes/deviations of setup a chroot on Ubuntu.

The
[stage3 tarball](http://www.gentoo.org/doc/en/handbook/handbook-amd64.xml?part=1&chap=2)
is "an archive containing a minimal Gentoo environment."
When extracting files from it,
you need root permission to preserve file permissions.
{% highlight bash %}
cd ${CHROOT}
sudo tar xvjpf stage3-*.tar.bz2
{% endhighlight %}

After extraction, make a `portage` directory inside chroot
since my host system is Ubuntu, which does not have that directory
{% highlight bash %}
sudo mkdir ${CHROOT}/usr/portage
{% endhighlight %}

I do not mount `/usr/portage` and `/usr/src/linux` for the same reason.
You may supply `-n` option when mounting directories
so that you do not write to `/etc/mtab`.

I followed the chapter 6 of
[Configuring Portage](://www.gentoo.org/doc/en/handbook/handbook-x86.xml?part=1&chap=6#doc_chap2)
and I skipped the other chapters.

I added myself with
{% highlight bash %}
# Inside Gentoo chroot
useradd -m -G users,wheel -u 1000 -s /bin/bash clchiou
{% endhighlight %}
Note that I set UID to be the same as my UID (1000) of the host system
so that I have access permissions of files inside chroot from the host system.

I added `USE="-X"` to `/etc/portage/make.conf`
since I do not need GUI inside chroot.

I installed `sudo` and
edited `/etc/sudoers` to allow users of `wheel` group running `sudo`

Finally, you may enter chroot from your host system with
{% highlight bash %}
sudo chroot ${CHROOT} sudo -i -u clchiou
{% endhighlight %}

For mounting and entering chroot, I took script excerpt from
[enter_chroot.sh](https://chromium.googlesource.com/chromiumos/platform/crosutils/+/master/sdk_lib/enter_chroot.sh)
{% gist c3e6d35fafbad06e729a %}
