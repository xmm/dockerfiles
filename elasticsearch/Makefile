SHELL=/bin/bash

ELASTIC_VERSION := "5.6.16"

ifndef VERSION_TAG
VERSION_TAG := $(ELASTIC_VERSION)
endif

ifndef DOCKER_REGISTRY
DOCKER_REGISTRY := xmmdev
endif

VERSIONED_IMAGE := $(DOCKER_REGISTRY)/elasticsearch:$(VERSION_TAG)

ifndef PLATFORM
PLATFORM := linux/arm64
#PLATFORM := linux/arm64,linux/amd64
endif

.PHONY: all clean pristine build buildx inspect push

all: build

clean:

pristine: clean
	docker rmi -f $(VERSIONED_IMAGE)

# Build docker image: "elasticsearch:$(VERSION_TAG)"
build: clean
	docker build -t $(VERSIONED_IMAGE) elasticsearch

buildx:
	docker buildx build --platform $(PLATFORM) -t $(VERSIONED_IMAGE) --push .

inspect:
	docker buildx imagetools inspect $(VERSIONED_IMAGE)

push:
	docker push $(VERSIONED_IMAGE)
