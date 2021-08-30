# This Dockerfile was generated from templates/Dockerfile.j2

FROM ubuntu:20.04

LABEL maintainer="Marat Khayrullin <xmm.dev@gmail.com>"

RUN apt-get update \
    && apt-get install -y locales \
    && apt-get install -y openjdk-8-jdk-headless \
    && apt-get install -y wget curl \
    && rm -rf /var/lib/apt/lists/* \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

ENV LANG en_US.utf8

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-arm64

ENV ELASTIC_CONTAINER true
ENV PATH /usr/share/elasticsearch/bin:$PATH
#ENV JAVA_HOME /usr/lib/jvm/jre-1.8.0-openjdk

RUN groupadd --gid 1000 elasticsearch && adduser --uid 1000 --gid 1000 --home /usr/share/elasticsearch elasticsearch

WORKDIR /usr/share/elasticsearch

USER elasticsearch

# Download and extract defined ES version.
RUN curl -fsSL https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.16.tar.gz | \
    tar zx --strip-components=1

RUN set -ex && for esdirs in config data logs; do \
        mkdir -p "$esdirs"; \
    done

# Install x-pack and also the ingest-{agent,geoip} modules required for Filebeat
RUN for PLUGIN in x-pack ingest-user-agent ingest-geoip analysis-icu analysis-kuromoji analysis-phonetic; do \
      elasticsearch-plugin install --batch "$PLUGIN"; \
    done
COPY elasticsearch.yml log4j2.properties config/
COPY x-pack/log4j2.properties config/x-pack/
COPY bin/es-docker bin/es-docker

USER root
RUN chown elasticsearch:elasticsearch \
      config/elasticsearch.yml \
      config/log4j2.properties \
      config/x-pack/log4j2.properties \
      bin/es-docker && \
    chmod 0750 bin/es-docker

USER elasticsearch
CMD ["/bin/bash", "bin/es-docker"]

EXPOSE 9200 9300
