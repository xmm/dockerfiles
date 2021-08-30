## Elasticsearch-5.6 docker build files for run on MacOS ARM M1 architecture

## Description

This repository contains the Docker build files generated from the official [elasticsearch-docker](https://github.com/elastic/elasticsearch-docker) repository (branch 5.6).

The files changed to use ubuntu:20.04 image, because image generated from centos7 takes 1.4G.

Added plugins: x-pack ingest-user-agent ingest-geoip analysis-icu analysis-kuromoji analysis-phonetic

## Required

- Docker
- Docker buildx
- Make

### Configure docker buildx for multi-architecture image builds.

Instructions:

- https://github.com/docker/buildx

- https://www.docker.com/blog/getting-started-with-docker-for-arm-on-linux/

## Build

```
make build
```
build image and push to docker.io registry with your own login name
```
DOCKER_REGISTRY=<your-docker-login> make build
```
or build specific architecture (default arm64) and push to docker.io registry
```
PLATFORM=linux/arm64 make buildx
```
