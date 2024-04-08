FROM frrouting/frr:v8.4.1

ENV DAEMONS="zebra bgpd ospfd isisd"
COPY ./daemons.conf /etc/frr/daemons.conf
RUN chown frr:frr /etc/frr/daemons.conf
