FROM arm32v6/alpine:3.10
LABEL io.balena.architecture="rpi"
 
LABEL io.balena.qemu.version="4.0.0+balena-arm"
COPY qemu-arm-static /usr/bin 

RUN apk add --no-cache --update \
  bash \
  ca-certificates \
  curl \
  dbus \
  findutils \
  tar \
  udev \
  gnupg \
  wget \
  && echo $'#!/bin/sh\n\
set -e\n\
set -u\n\
n=0\n\
max=2\n\
until [ $n -gt $max ]; do\n\
  set +e\n\
  (\n\
    apk add --no-cache "$@"\n\
  )\n\
  CODE=$?\n\
  set -e\n\
  if [ $CODE -eq 0 ]; then\n\
    break\n\
  fi\n\
  if [ $n -eq $max ]; then\n\
    exit $CODE\n\
  fi\n\
  echo "apk failed, retrying"\n\
  n=$(($n + 1))\n\
done' > /usr/sbin/install_packages \
  && chmod 0755 "/usr/sbin/install_packages"

RUN curl -SLO "http://resin-packages.s3.amazonaws.com/resin-xbuild/v1.0.0/resin-xbuild1.0.0.tar.gz" \
  && echo "1eb099bc3176ed078aa93bd5852dbab9219738d16434c87fc9af499368423437  resin-xbuild1.0.0.tar.gz" | sha256sum -c - \
  && tar -xzf "resin-xbuild1.0.0.tar.gz" \
  && rm "resin-xbuild1.0.0.tar.gz" \
  && chmod +x resin-xbuild \
  && mv resin-xbuild /usr/bin \
  && ln -sf resin-xbuild /usr/bin/cross-build-start \
  && ln -sf resin-xbuild /usr/bin/cross-build-end

ENV UDEV on

RUN adduser --system -u 1000 deluge

# Deluge version.
ARG DELUGE_VERSION=2.0.3

# Install Deluge.
WORKDIR /
#RUN wget http://download.deluge-torrent.org/source/deluge-${DELUGE_VERSION}.tar.gz
RUN wget http://download.deluge-torrent.org/source/2.0/deluge-${DELUGE_VERSION}.tar.xz
#RUN tar -zxvf deluge-${DELUGE_VERSION}.tar.gz
RUN tar -zxvf deluge-${DELUGE_VERSION}.tar.xz
#RUN rm deluge-${DELUGE_VERSION}.tar.gz
RUN rm deluge-${DELUGE_VERSION}.tar.xz
RUN cd deluge-${DELUGE_VERSION}; python setup.py build; python setup.py install

COPY start.sh /start.sh
COPY entry.sh /usr/bin/entry.sh
ENTRYPOINT ["/usr/bin/entry.sh"]
ENTRYPOINT ["/start.sh"]

RUN curl -SLO "https://raw.githubusercontent.com/balena-io-library/base-images/a95300eda2320833e537ca20d728a870bf02177d/scripts/assets/tests/test-os.sh" \
  && echo "Running test-os" \
  && chmod +x test-os.sh \
  && bash test-os.sh alpine 3.10 \
  && rm -rf test-os.sh



# Sets up a Deluge server and web client.

#FROM resin/rpi-raspbian:jessie
#MAINTAINER Jordan Crawford <jordan@crawford.kiwi>

# Install required packages.
#RUN apt-get update; apt-get install wget python python-twisted python-openssl python-setuptools intltool python-xdg python-chardet geoip-database python-libtorrent python-notify python-pygame python-glade2 librsvg2-common xdg-utils python-mako -y

# Setup a user.
#RUN adduser --system -u 1000 deluge

# Clean up.
#RUN apt-get clean
#RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Deluge version.
#ARG DELUGE_VERSION=1.3.13

# Install Deluge.
#WORKDIR /
#RUN wget http://download.deluge-torrent.org/source/deluge-${DELUGE_VERSION}.tar.gz
#RUN tar -zxvf deluge-${DELUGE_VERSION}.tar.gz
#RUN rm deluge-${DELUGE_VERSION}.tar.gz
#RUN cd deluge-${DELUGE_VERSION}; python setup.py build; python setup.py install

# Expose the deluge control port and the web UI port.
#EXPOSE 58846 8112

# Setup volumes.
#VOLUME /config
#VOLUME /data

# Add the start script.
#ADD start.sh /start.sh

# Run the start script on boot.
#CMD ["/start.sh"]
