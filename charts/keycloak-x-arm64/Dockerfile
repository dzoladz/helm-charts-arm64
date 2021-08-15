# --------------------------------------------------------------------
# Keycloak X - for arm64 k3s-based raspberry pi cluster
# --------------------------------------------------------------------
FROM ubuntu:20.04 AS build-env

ENV KEYCLOAK_VERSION 15.0.1
ARG KEYCLOAK_DIST=https://github.com/keycloak/keycloak/releases/download/$KEYCLOAK_VERSION/keycloak.x-preview-$KEYCLOAK_VERSION.tar.gz

RUN apt-get update && apt-get -y install \
    nano \
    tar \
    gzip \
    curl

RUN mkdir /opt/jboss \
    && cd /opt/jboss \
    && curl -L $KEYCLOAK_DIST | tar zx \
    && mv keycloak.x* keycloak

ADD tools /opt/jboss/tools

FROM ubuntu:20.04

COPY --from=build-env /opt/jboss /opt/jboss
COPY probes /probes

RUN apt-get update && apt-get -y install \
    openjdk-11-jre-headless \
    && apt-get clean \
    && rm -rf /var/cache/apt/* \
    && echo "jboss:x:0:root" >> /etc/group \
    && echo "jboss:x:1000:0:JBoss user:/opt/jboss:/sbin/nologin" >> /etc/passwd \
    && chown -R jboss:root /opt/jboss \
    && chmod -R g+rwX /opt/jboss

USER 1000

EXPOSE 8080
EXPOSE 8443

ENTRYPOINT [ "/opt/jboss/tools/docker-entrypoint.sh" ]