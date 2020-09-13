FROM arm32v7/debian

RUN apt-get update \
	&& apt-get -y install burp gettext-base

RUN rm -rf /etc/burp/*

ENV BURP_SERVER=burp \
    BURP_SERVER_PORT=4971 \
    BURP_CLIENT_PASSWORD=BurpClient123

VOLUME [ "/etc/burp" ]

COPY wait-for-it.sh /usr/local/bin/wait-for-it.sh
COPY burp.conf.tpl /etc/burp/burp.conf.tpl
COPY docker-cmd.sh /docker-cmd.sh

RUN chmod +x /usr/local/bin/wait-for-it.sh

ENTRYPOINT [ "/docker-cmd.sh" ]
CMD [ "-a", "t" ]
