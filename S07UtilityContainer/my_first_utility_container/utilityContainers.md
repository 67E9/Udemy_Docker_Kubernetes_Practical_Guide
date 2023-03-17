__BASICS__
How to run commands in a detached docker container? (examples)
- docker exec -it <container-name> npm init (RUN COMMAND IN RUNNING DETACHED CONTAINER)
- docker run -it <container-name> npm init (RUN CONTAINER AND OVERRITE COMMANDS SPECIFIED IN CMD)

__RUN THE EXAMPLE CONTAINER IN COMMAND LINE__

docker run -it --rm -v %cd%:/app node-util init

__RUN THE EXAMPLE CONTAINER IN DOCKER COMPOSE__

NOTE: docker compose exec will not help here

USE:
docker-compose run npm init