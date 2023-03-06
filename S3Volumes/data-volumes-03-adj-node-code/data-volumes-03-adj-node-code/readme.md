__start container with volume__

__anonymous volume__
Dockerfile:
VOLUME [ "path/inside/container" ]

=> this alone will create an __anonymous volume__
=> anonymous volume only exist as long at their container runs

__named volumes__

option: -v <name>:/path/to/folder/in/project

=> created when you run a container:
=> hidden somewhere, managed by docker
=> persistant, but not editable

remove volumes:

docker volume rm VOL_NAME
docker volume prune

__bind mount__

option: -v "/absolute/path/on/host":/path/to/folder/in/project

shortcuts:

macOS / Linux: -v $(pwd):/app

Windows: -v "%cd%":/app

on max/linux make sure docker hass access rights to the folder

=> persitant
=> editable
=> good for code you are working on
=> overwrites folder in image with local folder
=> beware, if node modules are inside the same folder a bind moutn will override node modules installed in docker image
=> to solve this: add an additional anonymous volume e.g. -v /app/node_modules => the longer internal path overrides subfolder of /app again

FINAL COMMAND with all three kinds of columes:

docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v %cd%:/app -v /app/node_modules <image-name:version>

docker logs <container-name> 
    =>see logs!

__read only__

use :ro to make a volume read-only, override subfolders of a read only volume with a more specific path, if they need to be written to!
(here feedback, node modules and temp are overridden)

docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "%cd%:/app:ro" -v /app/temp  -v /app/node_modules <image-name:version>

__manage volumes__

docker volumes ls => list active volumes, bind mounts don't show up
docker volume create => create volume by itself
docker volume inspect => see details about volume
docker volume rm => delete one volume
docker volume prune => delete all alused volumes

__runtime environment variables__

configure values from outside (for runtime)

in Dockerfile:
ENV KEY value

in commandline:
docker run --env KEY=value ...

in node-js-code: access via process.env.KEYg

__build time arguments__

configure default values at build time
with tags this allows you to build different images with different default values based on one code base

in Dockerfile
ARG KEY=value

in commandline
docker build --build-arg KEY=value