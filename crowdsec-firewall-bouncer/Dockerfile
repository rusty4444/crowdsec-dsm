FROM debian:bookworm-slim

RUN echo "nameserver 1.1.1.1" > /etc/resolv.conf \
    && apt-get update \
    && apt-get install -y --no-install-recommends ipset iptables \
    && rm -rf /var/lib/apt/lists/* \
    && update-alternatives --set iptables /usr/sbin/iptables-legacy \
    && update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy

# Install wrapper scripts that strip unsupported -m comment args
# (DSM 6.x kernel 4.4.302 lacks xt_comment module)
COPY iptables-wrapper /usr/sbin/iptables-wrapper
COPY ip6tables-wrapper /usr/sbin/ip6tables-wrapper

COPY bouncer-bin /usr/local/bin/crowdsec-firewall-bouncer

RUN chmod +x /usr/local/bin/crowdsec-firewall-bouncer \
             /usr/sbin/iptables-wrapper \
             /usr/sbin/ip6tables-wrapper \
    && mv /usr/sbin/iptables-wrapper /usr/sbin/iptables \
    && mv /usr/sbin/ip6tables-wrapper /usr/sbin/ip6tables

ENTRYPOINT ["crowdsec-firewall-bouncer", "-c", "/etc/crowdsec/bouncers/crowdsec-firewall-bouncer.yaml"]
