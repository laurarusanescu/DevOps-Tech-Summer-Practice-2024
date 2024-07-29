# Building an Docker Container from scratch

Create a new file /root/Dockerfile to build a container image from. It should:

    use bash as base
    run ping killercoda.com

Build the image and tag it as pinger .

Run the image (create a container) named my-ping .

First off we create and acces the Dockerfile file

#### vim Dockerfile

Then we add in the file

#### FROM bash
#### CMD ["ping", "killercoda.com"]


After that we build the image: 

#### docker build -t pinger
#### docker image ls

Run the image:

#### docker run --name my-ping pinger


### Pushing an image to the registry:

## Prentru a iesi dintr-un container initializat apasam CTRL+C.

Tag the image, which is currently tagged as pinger , also as local-registry:5000/pinger .

Then push the image into the local registry.

**docker tag pinger local-registry:5000/pinger**

**docker image ls**

**docker push local-registry:5000/pinger**

## Push image with custom tag to registry

Without specifying a :tag , the default :latest will be used. Now we want to use tag :v1 instead.

Tag the image, which is currently tagged as pinger , also as pinger:v1 and local-registry:5000/pinger:v1 .

Then push the image into the local registry.

**docker tag pinger pinger:v1**

**docker tag pinger local-registry:5000/pinger:v1**

**docker image ls**

**docker push local-registry:5000/pinger:v1**

# Using CMD and ENTRYPOINT in Dockerfile

## Using CMD

Build image cmd-echo using /root/Dockerfile . Check what is CMD of the newly created cmd-echo image.

Run the cmd-echo container:

    Don't modify Dockerfile
    Overwrite CMD with date via CLI
    Set the ENTRYPOINT to date via CLI

There can only be one CMD instruction in a Dockerfile.
If you list more than one CMD, only the last one takes effect.

The purpose of a CMD is to provide defaults for an executing container.

However, it can be used as a way to provide an executable and defaults.

Solution:

Build docker image /root/Dockerfile :

docker build -t cmd-echo .


Explore CMD of cmd-echo :

docker inspect cmd-echo | jq .[0].ContainerConfig.Cmd


Run the container with default values:

docker run --rm cmd-echo


Run the container with updated CMD command:

docker run --rm cmd-echo date


Run the container with ENTRYPOINT date :

docker run --rm --entrypoint date cmd-echo

## Using ENTRYPOINT

Remove CMD command from Dockerfile and add ENTRYPOINT command that will run the same echo hi, from container! command.

Build image entrypoint-echo using /root/Dockerfile. Check what is ENTRYPOINT  of the newly created entrypoint-echo image.

Run entrypoint-echo container:
1. Overwrite ENTRYPOINT with date via CLI
2. Set CMD to date via CLI

You can use the ENTRYPOINT to set fairly stable default commands 
and arguments and then use either form of CMD to set 
additional defaults that are more likely to be changed.


Solution:

Modify /root/Dockerfile :

cat > /root/Dockerfile <<EOF
FROM alpine
ENTRYPOINT ["echo", "hi, from container!"]
EOF


Build docker image /root/Dockerfile :

docker build -t entrypoint-echo .


Explore ENTRYPOINT of entrypoint-echo :

docker inspect entrypoint-echo | jq .[0].ContainerConfig.Entrypoint


Run the container with default values:

docker run --rm entrypoint-echo


Run the container with date:

docker run --rm --entrypoint date entrypoint-echo


Run the container with entrypoint date :

docker run --rm entrypoint-echo date

## Using Entrypoint and CMD

Use CMD and ENTRYPOINT to run the same echo "hi, from container!" command. Build image from /root/Dockerfile and name it image-echo . Check what is CMD and ENTRYPOINT of the newly created image-echo image.

Run the image-echo container:

    with default cmd and entrypoint values
    with another message hi, from the updated image via CLI
    Set the CMD to date via CLI
    Set ENTRYPOINT to date via CLI

In essence, the entrypoint is a binary, and cmd contains parameters in Docker. When running a container, you have the flexibility to override both entrypoint and cmd parameters.

However, overwriting the command is simpler when the entrypoint is either not defined or set as a shell. Therefore, for a more specific image, consider specifying the ENTRYPOINT along with default parameters through CMD. These parameters can then be easily modified via CLI when executing 'docker run.'

Alternatively, if you prefer, you can leave the ENTRYPOINT as is (often just a shell) and provide the complete command in the CMD. This allows you to use the image seamlessly to construct containers with any desired command.

Modify /root/Dockerfile :

cat > /root/Dockerfile <<EOF
FROM alpine
ENTRYPOINT ["echo"]
CMD ["hi, from container!"]
EOF


Build docker image /root/Dockerfile :

docker build -t image-echo .


Explore CMD and ENTRYPOINT of image-echo :

docker inspect image-echo | jq .[0].ContainerConfig.Cmd &&
docker inspect image-echo | jq .[0].ContainerConfig.Entrypoint


Run the container with default values:

docker run --rm image-echo


Run the container with updated message:

docker run --rm image-echo hi, from the updated image


Run the container with CMD date :

docker run --rm image-echo date


Run the container with ENTRYPOINT date :

docker run --rm --entrypoint date image-echo

# Dockerfile best practices

Layers

The order of Dockerfile instructions matters. A Docker build consists of a series of ordered build instructions. Each instruction in a Dockerfile roughly translates to an image layer. The following diagram illustrates how a Dockerfile translates into a stack of layers in a container image.

In Dockerfile every command is a layer. And if command is doing exactly the same then the layer will be loaded from cache.
However, in our case we copy all of the files (dependecies and modified source code files) in one command.
More effective will be to initially copy dependecy files (as they are changed less often than source code) and install them.
And only afterwards copy source code files. This way we got more layers loaded from the cache on the build of a new image.
It seems like a very small gain in time here, but it can be important when building really big applications.

## Cached Layers

There is a Dockerfile /root/app/Dockerfile. Modify it, so dependency layer can be reused. Build the image, name it server-1

Solution:


Modify /root/app/Dockerfile (remove # cached comments first):

# syntax=docker/dockerfile:1
FROM golang:1.21-alpine # cached
WORKDIR /src # cached
COPY go.mod go.sum /src/ # cached
RUN go mod download # cached
COPY . .
RUN go build -o /bin/client ./cmd/client
RUN go build -o /bin/server ./cmd/server
ENTRYPOINT [ "/bin/server" ]


Build the image:

docker build -t server-1 .

# Multi-stage build

Separate build stage and create image only with the resulted binary. Compare the size of the images server-1 and server-2 . Use scratch as a base image for the runnable image.

There are two main reasons for why you’d want to use multi-stage builds:
* They allow you to run build steps in parallel, making your build pipeline faster and more efficient.
* They allow you to create a final image with a smaller footprint, containing only what's needed to run your program.

With multi-stage builds:
- you can run builds in parallel
- you can separate build files from binaries

Solution:

(# syntax=docker/dockerfile:1)
FROM golang:1.21-alpine
WORKDIR /src
COPY go.mod go.sum /src/
RUN go mod download
COPY . .
RUN go build -o /bin/client ./cmd/client
RUN go build -o /bin/server ./cmd/server
(# as it is just a build step, we don't need CMD or ENTRYPOINT command)

FROM scratch # minimal base image
COPY --from=0 /bin/server /bin/ # copy /bin/server file from the previously build image (0 as we didn't name it)
ENTRYPOINT [ "/bin/server" ]


Build the image:

docker build -t server-2 .

# Build multiple images

Build **run-server** and **run-client** images from the same Dockerfile using target keyword.

It’s possible to create multiple different images using a single Dockerfile. 
You can specify a target stage of a build using the --target flag. 
Replace the unnamed FROM scratch stage with two separate stages named client and server.

Slution:

Add next line to the /root/Dockerfile :

# syntax=docker/dockerfile:1
FROM golang:1.21-alpine AS base
WORKDIR /src
COPY go.mod go.sum /src/
RUN go mod download
COPY . .

# build client
FROM base AS build-client
RUN go build -o /bin/client ./cmd/client

# build server
FROM base AS build-server
RUN go build -o /bin/server ./cmd/server

# copy client binary to client image
FROM scratch AS client
COPY --from=build-client /bin/client /bin/
ENTRYPOINT [ "/bin/client" ]

# copy server binary to server image
FROM scratch AS server
COPY --from=build-server /bin/server /bin/
ENTRYPOINT [ "/bin/server" ]


Build the client image:

docker build -t run-client --target=client .


Build the server image:

docker build -t run-server --target=server .


List available images:

docker image ls


## Build arguments

Add build argument GO_VERSION to the Dockerfile. And build image with provided golang version 1.22.

Documentation: https://docs.docker.com/build/guide/build-args/

Build arguments is a great way to add flexibility to your builds. 
You can pass build arguments at build-time, and you can set a default value that the builder uses as a fallback.

You can also inject values inside your code, but the way it's done is different for every language.

Solution: 


Add next line to the /root/Dockerfile :

# syntax=docker/dockerfile:1

# defining variable
ARG GO_VERSION=1.21

# using variable
FROM golang:${GO_VERSION}-alpine AS base
WORKDIR /src
COPY go.mod go.sum /src/
RUN go mod download
COPY . .

FROM base AS build-client
RUN go build -o /bin/client ./cmd/client

FROM base AS build-server
RUN go build -o /bin/server ./cmd/server

FROM scratch AS client
COPY --from=build-client /bin/client /bin/
ENTRYPOINT [ "/bin/client" ]

FROM scratch AS server
COPY --from=build-server /bin/server /bin/
ENTRYPOINT [ "/bin/server" ]


Build the image:

docker build --build-arg="GO_VERSION=1.22" .

# Export binaries

Export binaries to the /root/app/bin as server and client using Dockerfile.

The default format of output for build command is docker image.
But, it can be changed to binary by using local exporter (use --output flag).

Use docker build -h

Solution:

Add next line to the /root/app/Dockerfile :

# syntax=docker/dockerfile:1
ARG GO_VERSION=1.21
FROM golang:${GO_VERSION}-alpine AS base
WORKDIR /src
COPY go.mod go.sum /src/
RUN go mod download
COPY . .

FROM base AS build-client
RUN go build -o /bin/client ./cmd/client

FROM base AS build-server
RUN go build -o /bin/server ./cmd/server

FROM scratch AS client
COPY --from=build-client /bin/client /bin/
ENTRYPOINT [ "/bin/client" ]

FROM scratch AS server
COPY --from=build-server /bin/server /bin/
ENTRYPOINT [ "/bin/server" ]

# exporting binaries from server and client images
FROM scratch AS binaries
COPY --from=server /bin/server /
COPY --from=client /bin/client /


Export binaries:

docker build --output=/root/app --target=binaries .


