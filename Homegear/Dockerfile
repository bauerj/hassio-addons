# https://github.com/Homegear/Homegear-Docker/blob/master/rpi-stable/Dockerfile
ARG BUILD_FROM=hassioaddons/ubuntu-base:2.2.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

MAINTAINER Johann Bauer <johann@bauer.pizza>
COPY qemu-arm-static /usr/bin/qemu-arm-static
RUN apt-get update && apt-get -y install apt-transport-https wget ca-certificates apt-utils gnupg libzip4 libavahi-client3 libavahi-client-dev
RUN \
    ln -s /bin/echo /usr/bin/insserv && \
    touch /tmp/HOMEGEAR_STATIC_INSTALLATION && \
    wget https://apt.homegear.eu/Release.key && apt-key add Release.key && rm Release.key && \
    echo 'deb https://apt.homegear.eu/Ubuntu/ bionic/' >> /etc/apt/sources.list.d/homegear.list && \
    apt-get update && apt-get -y install homegear homegear-management homegear-webssh homegear-adminui homegear-nodes-core homegear-nodes-extra homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-kodi homegear-ipcam homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-zwave homegear-mbus homegear-ccu homegear-influxdb && \
    rm -f /etc/homegear/dh1024.pem && \
    rm -f /etc/homegear/homegear.crt && \
    rm -f /etc/homegear/homegear.key && \
    cp -R /etc/homegear /etc/homegear.config && \
    rm -rf /etc/homegear && \
    sed -i "s/interface = ::1/interface = 0.0.0.0/g" /etc/homegear.config/rpcservers.conf && \
    cp -R /var/lib/homegear /var/lib/homegear.data && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY start.sh /start.sh
RUN chmod +x /start.sh
ENTRYPOINT ["/bin/bash", "-c", "/start.sh"]

EXPOSE 2001 2002 2003
