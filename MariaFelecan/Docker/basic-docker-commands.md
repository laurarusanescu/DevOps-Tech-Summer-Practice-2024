we build the image by:
docker build -t app-image .

run container with default commands:
docker run --rm app-image

to inspect the cmd of an image named 'cmd-echo' : 
docker inspect cmd-echo | jq .[0].ContainerConfig.Cmd

