FROM    debian:stable-slim

LABEL   maintainer="Oliver Friesen<oliver.friesen@gmail.com>"

ENV     DEBIAN_FRONTEND noninteractive

RUN     apt-get update && \
	    apt-get  --quiet --yes --no-install-recommends install \
			apt-utils \
			git-core \
			build-essential \
			fakeroot \
			debhelper \
			autotools-dev \
			autoconf \
			automake \
			libtool \
			libusb-1.0-0-dev \
			pkg-config \
			libsystemd-dev \
			dh-systemd \
			libev-dev \
			libsystemd-dev \
			cmake \
			wget \
			cdbs \
			ca-certificates \
			nano \
			subversion			

RUN     adduser --system --disabled-password someuser

USER	someuser
