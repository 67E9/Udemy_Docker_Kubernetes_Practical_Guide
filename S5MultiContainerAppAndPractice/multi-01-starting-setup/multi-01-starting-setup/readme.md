docker network create goals-net

__dockerize mongo db__

docker run -d --name mongo-goals -v mongo-goals:/data/db --network goals-net -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --rm mongo:latest

NOTE: username and password must be added to db-connection-string in backend, ref. documentation

__dockerize backend__

write Dockerfile

docker build -t backend-goals backend/.

docker run -d -p 80:80 --name backend-goals -v %cd%/backend:/app -v /app/node_modules --network goals-net --rm backend-goals

NOTE: db and backend communicate via the docker network, use mongo-goals as hostname in node code
NOTE: mongo db credentials can be set in this example using --env MONGO_USR=value and --env MONGO_PW=value
NOTE: added nodemon for reload

__dockerize frontend__

write Dockerfile

docker build -t frontend-goals frontend/.

docker run -it --rm --name frontend-goals -p 3000:3000 -v %cd%/frontend/src:/app/src frontend-goals

DEV SERVER FOR REACT NEEDS -it

NOTE: backend and frontend cannot communicate via docker networking, because the react code does not run on the dev server in the container but in the browser, to allow browser code to access the endpoints via localhost, port 80 must be published for backend app (-p 80:80) 
NOTE: nodemon not needed as react uses webkit already
NOTE: hot reload currently does not work on windows filesystem  &#128580;