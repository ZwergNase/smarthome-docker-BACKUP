FROM    oliverf/builder as builder

LABEL   maintainer="Oliver Friesen <oliver.friesen@gmail.com>" 

ENV     DEBIAN_FRONTEND noninteractive

RUN     svn --non-interactive --trust-server-cert co https://svn.fhem.de/fhem/trunk/fhem /tmp/fhem && \
		svn info /tmp/fhem | sed -n -E 's/Revision: (.*)/---> tag ---> \1/p' && \
		svn info /tmp/fhem | sed -n -E 's/Revision: (.*)/---> label ---> main-vcs-ref="\1"/p' && \
		svn info /tmp/fhem | sed -n -E 's/^URL: (.*)/---> label ---> main-vcs-url="\1"/p' && \
		svn info /tmp/fhem | sed -n -E 's/Last Changed Date: ([0-9-]+) ([0-9:]+).*/---> label ---> main-build-date="\1T\2Z"/p'

FROM    debian:stable-slim

ENV     DEBIAN_FRONTEND noninteractive
ENV     TZ Europe/Berlin

RUN     apt-get update && \
        apt-get --quiet --yes --no-install-recommends install \
			tzdata \
			usbutils \
			perl \
			libtime-hires-perl \
			libdevice-serialport-perl \
			libjson-perl \
			libio-socket-ssl-perl \
		    libpath-tiny-perl \
	        liblinux-inotify2-perl \            
        	libdata-dump-perl \             
            libfile-find-rule-perl \  
			libcrypt-rijndael-perl \
			libdatetime-format-strptime-perl && \
		apt-get -q -y clean && \
		rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

RUN		addgroup fhem && \
		adduser --system --ingroup fhem --disabled-password --uid 1000 fhem && \
		usermod -a -G dialout fhem

WORKDIR /opt/fhem/

COPY	--from=builder --chown=fhem:fhem /tmp/fhem .
RUN     chown -R fhem:fhem ../fhem

COPY	--chown=fhem:fhem fhem.cfg /home/fhem/fhem.cfg

USER	fhem

RUN		sed -n -E 's/PRETTY_NAME=(.*)/---> label ---> os-version=\1/p' /etc/os-release && \
		date -u +'---> label ---> os-last-update="%Y-%m-%dT%H:%M:%SZ"'		

RUN     mv log ~/log && \
    	mv www/gplot ~/gplot && \
        ln -s ~/log log && \
	    ln -s ~/gplot www/gplot		

EXPOSE  8083 8084 8085 7072

CMD     ["perl", "fhem.pl", "/home/fhem/fhem.cfg"]
