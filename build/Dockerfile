FROM debian:bookworm-20240130-slim

COPY non-free.list /etc/apt/sources.list.d/

RUN apt-get update && \
    apt-get --yes install \
        snmptrapd \
        snmp-mibs-downloader && \
    mkdir /usr/share/snmp/mibs/cisco

COPY snmp.conf snmptrapd.conf /etc/snmp/
COPY mib/*.txt /usr/share/snmp/mibs/cisco/

ENTRYPOINT ["snmptrapd", "-f"]

EXPOSE 162/udp
