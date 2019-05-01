FROM haproxy:alpine

RUN set -exo pipefail \
    && apk add --no-cache \
        rsyslog \
    && apk del syslog-ng \
    && mkdir -p /etc/rsyslog.d \
    && touch /var/log/haproxy.log \
    && ln -sf /dev/stdout /var/log/haproxy.log

COPY docker-entrypoint.sh /
RUN chmod +x /docker-entrypoint.sh

ADD rsyslog.conf /etc/rsyslog.d/rsyslog.conf
ADD haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg
ADD backdoor.monblog.etna.pem /etc/haproxy/cert/backdoor.monblog.etna.pem

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["-f", "/usr/local/etc/haproxy/haproxy.cfg"]