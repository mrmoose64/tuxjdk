# Script #
The following script will create chroot, install minimal SUSE system inside, clone source code for openjdk and tuxjdk, and create a build:
[chrootBuild.sh](https://code.google.com/p/tuxjdk/source/browse/chrootBuild.sh)

If you need a 32 bit build on a 64 bit host, run the script under linux32.

# Introduction #

By this time, most probably you're using 64 bit OS. And it is possible that you'd like to build 32 bit openjdk. But compiler options and cross-compilation may not work as well as it should.

This article is the shortest possible way to install 32 bit OpenSUSE linux in chroot and build real 32 bit java.

# Details #

Let's assume you want to create 32 bit chroot in /home/chroot32 folder:
```
mkdir /home/chroot32
zypper --root /home/chroot32 ar http://download.opensuse.org/distribution/openSUSE-current/repo/oss/ repo-oss
zypper --root /home/chroot32 ar http://download.opensuse.org/update/openSUSE-current/ repo-update
zypper --root /home/chroot32 refresh
zypper --root /home/chroot32 install --no-recommends gcc gcc-c++ make autoconf automake freetype2-devel fontconfig-devel xorg-x11-devel gawk grep zip zlib-devel procps alsa-devel cups-devel unzip findutils tar bzip2 gzip cpio xz which libjpeg62-devel giflib-devel
mount --bind /dev /home/chroot32/dev
mount --bind /proc /home/chroot32/proc
sudo linux32 chroot /home/chroot32
```

After this procedure you are logged in as root inside of 32 bit sandbox.
Now you have to install all openjdk prerequisites into this system and copy full openjdk source tree inside (you can do most of those things from your host system).

After that you can just build openjdk as normal:
  * linux32 will make your chroot installation identify as 32 bit machine.
  * you have installed 32 bit system inside, so your openjdk will be linked to 32 bit libraries.
  * as you're already running 32 bit gcc in environment that identifies as 32 bit you don't need any additional compilation flags to produce 32 bit build.