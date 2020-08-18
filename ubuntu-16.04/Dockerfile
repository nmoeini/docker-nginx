FROM ubuntu:xenial-20200706

LABEL name="nmoeini/nginx:1.16-xenial-20200706" mainteiner="Navid Moeini <nmoeiny@yahoo.com>" version="0.0"

ARG DEBIAN_FRONTEND

ENV DEBIAN_FRONTEND=${DEBIAN_FRONTEND:-noninteractive}

## Install tools and dependencies  \
RUN set -eux \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        software-properties-common \
        gnupg \
        dirmngr \
        vim \
        curl \
        wget \
        \
## Clean up \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

## Install nginx  \
RUN set -eux \
    && apt-add-repository -y ppa:nginx/stable \
    && apt-get update \
    && apt-get install -y --no-install-recommends nginx \
    \
## Clean up \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ADD conf/nginx-default.conf.tmpl /etc/nginx/conf.d/default.conf

ADD data/www/* /var/wwww/html/

## forward request and error logs to docker log collector
RUN set -eux \
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log


STOPSIGNAL SIGTERM

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
