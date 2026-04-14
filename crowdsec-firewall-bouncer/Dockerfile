FROM debian:bookworm-slim

RUN echo "nameserver 1.1.1.1" > /etc/resolv.conf \
    && apt-get update \
    && apt-get install -y --no-install-recommends ipset iptables \
    && rm -rf /var/lib/apt/lists/* \
    && update-alternatives --set iptables /usr/sbin/iptables-legacy \
    && update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy

COPY bouncer-bin /usr/local/bin/crowdsec-firewall-bouncer
RUN chmod +x /usr/local/bin/crowdsec-firewall-bouncer

ENTRYPOINT ["crowdsec-firewall-bouncer", "-c", "/etc/crowdsec/bouncers/crowdsec-firewall-bouncer.yaml"]
