# Docker CI Images
A collection of images that can be used in CI pipelines based on docker.

* CircleCI
* GitLab-CI
* ...

## Purpose
Instead of installing the dependencies on every run and possibly using 
different images for every stage (which is ok, don't get me wrong), 
you can use the image that has all the tools you need for your pipeline.

Included tools:
* `git`, `curl`, `jq`

## Build multi-arch docker images
Use `dind-buildx`:
* based on `docker:dind`
* includes `buildx` so you can build for multiple architectures

### Usage
* Start docker engine with `dind dockerd &` (if not using remote docker engine)
* bootstrap your `buildx` with
```shell
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
docker context create xbuilder
docker buildx create xbuilder --name xbuilder --use
docker buildx inspect --bootstrap
```
* use `docker` and `docker buildx` as usual

### Examples
* Circle CI: https://github.com/DrPsychick/docker-influxdb/blob/master/.circleci/config.yml

## Build images and deploy with helm
Use `dind-buildx-helm`:
* based on `drpsychick/dind-buildx`
* includes `kubectl` and `helm` so you can build your image and deploy with kubectl and/or helm

### Usage
Same as above plus:
* provide kubernetes config and point to it with `$KUBECONFIG`
* use `kubectl` and `helm` to deploy to your kubernetes cluster

## Build images, test helm charts and deploy to kind
Use `dind-buildx-helm-kind`:
* based on `drpsychick/dind-buildx-helm`
* includes `ct` and `kind` so you can test your helm chart and deploy it to a kind node

### Usage
Same as above plus:
* start a kind cluster with `kind create cluster`
* optionally forward localhost to docker engine host, if using remote docker engine
* test install your charts using `ct`

