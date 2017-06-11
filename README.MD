# Web Microservice

Micro Container Web Service developed in GO. This is a small tutorial on how to create Micro containers (< 5 mb) running a Web server in Go language.

## Why GO?

* Very simple, elegant and performant language
* Building process generates small binaries ideal for micro containers
* Fun to learn

## Why Docker EDGE?

* Supports multi stage builds. This means in the same Dockerfile multiple images can be generated, this is ideal for creating first a builder container for your sourcecode which then generates an artifact (binary) that will be the content of our final "production ready" image, very lean.

## Prerrequisites

* Download and install [Docker](https://docs.docker.com/docker-for-mac/install/) (Multi Stage Dockerfile support needed, Docker EDGE recommended)

## Getting Started

These instructions will get you running the code from go and building the images from Docker.

Download the source
```
go get github.com/gianksp/websrv
```

Run server

```
cd ~/go/src/github.com/gianksp/websrv
go run main.go
```

Visit localhost:8080 on your browser to check the time

### Dockerizing

Now it's time to generate our Micro containers

From the root of the websrv project
```
cd ~/go/src/github.com/gianksp/websrv
```

Build the docker server images
```
docker build . -f Dockerfile.server --rm=true -t web/service:1.0 ; docker rmi $(docker images -f "dangling=true" -q)
```

The commands above generate the following images. A couple of beautiful 10Mb images for the server and the client
![Micro containers](http://i.imgur.com/nUmqH1A.png)

Run the docker server image assigning a static ip within the previous network
```
docker run -it -p 8080:8080 --name web_service web/service:1.0
```
