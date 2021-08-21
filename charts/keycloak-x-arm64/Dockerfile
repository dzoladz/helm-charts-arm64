# --------------------------------------------------------------------
# Keycloak - for arm64 k3s-based raspberry pi cluster
# --------------------------------------------------------------------
FROM ubuntu:20.04

ENV KEYCLOAK_VERSION 15.0.1
ARG KEYCLOAK_DIST=https://github.com/keycloak/keycloak/releases/download/$KEYCLOAK_VERSION/keycloak-$KEYCLOAK_VERSION.tar.gz

RUN apt-get update && apt-get -y install \
    nano \
    tar \
    gzip \
    curl \
    openjdk-11-jre-headless \
    && apt-get clean \
    && rm -rf /var/cache/apt/*

RUN cd /usr/local \
    && curl -L $KEYCLOAK_DIST | tar -xz \
    && mv keycloak-* keycloak

# 😂 Admin User
COPY keycloak-add-user.json /usr/local/keycloak/standalone/configuration/keycloak-add-user.json

EXPOSE 8080
EXPOSE 8443
EXPOSE 9990

CMD ["/usr/local/keycloak/bin/standalone.sh", "-b", "0.0.0.0"]