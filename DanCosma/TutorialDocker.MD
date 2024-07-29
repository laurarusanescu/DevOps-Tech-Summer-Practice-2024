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

