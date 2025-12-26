# @description reference from https://github.com/yobasystems/alpine-mariadb/tree/main
# ------------------------------------------------------------------------------------
# @link https://hub.docker.com/_/alpine/      alpine image
# @link https://hub.docker.com/_/mariadb/     mariadb image
# ------------------------------------------------------------------------------------
# @build-example docker build . -f Dockerfile -t passkey/mariadb:11-alpine
#                docker build --build-arg app_env=test . -f Dockerfile -t passkey/mariadb:11-alpine
# ------------------------------------------------------------------------------------
# @run-container docker run -d -it --name tmariadb passkey/mariadb:11-alpine
#                docker exec -it tmariadb /bin/sh
# ------------------------------------------------------------------------------------

FROM alpine:3.23
LABEL maintainer="gengwh <8505569@qq.com>" version="1.0"

##
# ---------- env settings ----------
##

# --build-arg timezone=Asia/Shanghai
ARG timezone
# prod grey test dev
ARG app_env=prod

ARG BUILD_DATE
ARG VCS_REF

ENV APP_ENV=${app_env:-"prod"} \
    TIMEZONE=${timezone:-"Asia/Shanghai"}


##
# ---------- building ----------
##

RUN set -ex \
        # change apk source repo
        && sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories \
        && apk update \
        && apk add --no-cache \
        # Install base packages
        mariadb \
        mariadb-client \
        mariadb-server-utils \
        pwgen \
        && apk del --purge *-dev \
        && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
        # - config timezone
        && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
        && echo "${TIMEZONE}" > /etc/timezone


COPY run.sh /scripts/run.sh
RUN mkdir /docker-entrypoint-initdb.d && \
    mkdir /scripts/pre-exec.d && \
    mkdir /scripts/pre-init.d && \
    mkdir /scripts/first-run.d && \
    chmod -R 755 /scripts

EXPOSE 3306

VOLUME ["/var/lib/mysql"]

ENTRYPOINT ["/scripts/run.sh"]









