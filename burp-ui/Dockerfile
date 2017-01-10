FROM fedora:25
MAINTAINER "Peter Schiffer" <pschiffe@redhat.com>

RUN dnf -y --setopt=tsflags=nodocs install \
        python3-pyOpenSSL \
        python3-gevent \
        python3-gunicorn \
    && dnf clean all

RUN pip3 install https://burpui.ziirish.me/builds/burp-ui.dev.tar.gz \
    && pip3 install envtpl "burp-ui[ldap_authentication]" \
    && rm -rf ~/.cache/* \
    && mkdir -p /etc/burp /etc/burp.tpl

EXPOSE 5000

VOLUME [ "/etc/burp" ]

COPY burpui.cfg.tpl /etc/burp.tpl/burpui.cfg.tpl
COPY docker-cmd.sh /docker-cmd.sh

CMD [ "/docker-cmd.sh" ]
