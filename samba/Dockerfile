FROM    debian:stable-slim

ENV     DEBIAN_FRONTEND noninteractive
ENV     TZ Europe/Berlin

RUN     apt-get update && \
        apt-get --quiet --yes --no-install-recommends install \
		tzdata \
		samba && \
	apt-get -q -y clean && \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

RUN	adduser --system --disabled-password --uid 1000 samba

WORKDIR	/home/samba

COPY	smb.conf start.sh ./

RUN	chmod og-wrx *

CMD [ "/bin/bash", "start.sh"]
