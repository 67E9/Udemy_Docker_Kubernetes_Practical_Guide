# choose base image to start from (herenode from dockerhub)
FROM node

#set working directory
WORKDIR /app

#first copy package.json, so npm install only runs, if package.json changed
COPY package.json /app

#RUN runs command in working directory by default
#runs when image is created
RUN npm install

# add files from folder that contains docker at /app in container
COPY . /app 
# because WORKDIR is /app . . or . /. would also work
# copy rest of files after npm install

#expose port 80 to local machine
#use -p when starting server, EXPOSE is only for documentation
EXPOSE 80

#runs when image is instantiated as container
CMD ["node", "server.js"]