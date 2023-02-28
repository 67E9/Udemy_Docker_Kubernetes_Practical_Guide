how to run this:
- on Windows start Docker Desktop with admin rights first
- docker build <path> => create image from fiels at path
- docker run -p 3000:80 <image-id> => to spin up container, default attached mode (-d for detached)
- docker ps => see all running containers

- docker ps -a => see all containers, even if not running

- docker start <container-id or name> => restart container, default detached mode (-a to start in attached mode)

- docker attach <container-id or name> => attach runnign cotnainer to this console

- docker logs <container-id or name> => get past logs printed by a cotnroller

- docker rm <container-id or name> <container-id or name> => remove stopped container

- docker images => list images
- docker rmi <image-id> 
    => delete image, only if not container uses them
- docker image prune => removes all unsused images
- docker image inspect <image-id> ==> get detailled info about image
-docker build --tag <name:tag>  (ALT: -t)
    => name images (repositoryName:tag), tagallows selecting specific version of image repository!

-docker run --rm <image id> => container will be removed on stopping it
- docker run --name <name> <image> => set custom container name

- docker cp <source> <destination>: copy files into or out of running container (destination: container:/path), beware of currently runnign files 

restart in interactive mode

- docker start  -a -i <container-id>