# Image: glerchundi/base:12.04
FROM ubuntu:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

# To allow automated installation of new packages
ENV DEBIAN_FRONTEND noninteractive

# Work around initramfs-tools running on kernel 'upgrade':
# <http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=594189>
ENV INITRD No

# Set correct environment variables.
ENV HOME /root

# Work around initscripts trying to mess with /dev/shm:
# <https://bugs.launchpad.net/launchpad/+bug/974584>
# Used by our `assets/ischroot` binary to behave in our custom way, to always
# say we are in a chroot.
ENV FAKE_CHROOT 1
RUN mv /usr/bin/ischroot /usr/bin/ischroot.original
ADD assets/ischroot /usr/bin/ischroot
RUN chmod +x /usr/bin/ischroot

# Configure no init scripts to run on package updates.
ADD assets/policy-rc.d /usr/sbin/policy-rc.d
RUN chmod +x /usr/sbin/policy-rc.d

# Normally dpkg will call fsync after every package is installed, but when
# building an image we don't need to worry about individual fsyncs, so we can
# use the force-unsafe-io option:
RUN echo 'force-unsafe-io' | tee /etc/dpkg/dpkg.cfg.d/02apt-speedup

# Since we want make the image as small as possible, we add a Post-Invoke hook
# to dpkg which deletes cached deb files after installation:
RUN echo 'DPkg::Post-Invoke {"/bin/rm -f /var/cache/apt/archives/*.deb || true";};' | tee /etc/apt/apt.conf.d/no-cache

# We don't need an apt cache in a container
RUN echo 'Acquire::http {No-Cache=True;};' | tee /etc/apt/apt.conf.d/no-http-cache

# Finally we run dist-upgrade:
RUN apt-get update -y && apt-get dist-upgrade -y

# Make sure the package repository is up to date
RUN apt-get -y install sudo wget vim pwgen vim curl net-tools unzip nmap iputils-* \
                       language-pack-en python-software-properties supervisor \
                       openssh-server

# versions and urls
ENV ETCD_VERSION 0.4.3
ENV ETCD_URL https://github.com/coreos/etcd/releases/download/v$ETCD_VERSION/etcd-v$ETCD_VERSION-linux-amd64.tar.gz
ENV CONFD_VERSION 0.4.0
ENV CONFD_URL https://github.com/kelseyhightower/confd/releases/download/v$CONFD_VERSION/confd-$CONFD_VERSION-linux-amd64

# etcd
RUN wget -O /opt/etcd-$ETCD_VERSION.tar.gz $ETCD_URL 
RUN tar xvzf /opt/etcd-$ETCD_VERSION.tar.gz -C /opt
RUN rm /opt/etcd-$ETCD_VERSION.tar.gz
RUN mv /opt/etcd-* /opt/etcd-$ETCD_VERSION && ln -s /opt/etcd-$ETCD_VERSION /opt/etcd
RUN ln -s /opt/etcd/etcdctl /usr/local/bin/etcdctl

# confd
RUN wget -O /usr/local/bin/confd $CONFD_URL
RUN mkdir -p /etc/confd/conf.d  /etc/confd/templates

# Fix locale
RUN locale-gen en_US.UTF-8
RUN dpkg-reconfigure locales

# Add custom app files
ADD assets/.vimrc /root/.vimrc
ADD assets/.bash_aliases /root/.bash_aliases
ADD assets/ssh-authorized-keys /usr/local/bin/ssh-authorized-keys
RUN chmod 755 /usr/local/bin/ssh-authorized-keys

# Set timezone based on TZ environment variable
ADD assets/timezone /usr/local/bin/timezone
RUN /usr/local/bin/timezone

# Add setup app files & install
ADD assets/setup/install /app/setup/install
RUN chmod 755 /app/setup/install
RUN /app/setup/install

# We want to clean out a few more cached files:
RUN apt-get clean && rm -rf /var/cache/apt/*
