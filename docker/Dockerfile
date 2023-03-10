ARG NGINX_VERSION
FROM nginx:${NGINX_VERSION}-alpine

LABEL maintainer="Oleg Sedinkin <a@aw.wtf>"

RUN set -ex \
    && apk upgrade --update-cache --available \
    && apk add \
            bash \
            openssl \
            git \
    && git clone https://github.com/neilpang/acme.sh.git acme_src \
    && cd /acme_src && ./acme.sh --install --cert-home /.acme.data \
    && apk del git \
    && rm -rf /acme_src /var/cache/apk/* \
    && mkdir /etc/nginx/ssl \
    && mkdir /etc/nginx/ssl/app \
    && mkdir /etc/nginx/ssl/default

COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/default.conf

COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh

WORKDIR /var/www

EXPOSE 80
EXPOSE 443

ENTRYPOINT ["/docker-entrypoint.sh"]
