FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

#build-time argument, set using --build-arg KEY=value
ARG DEFAULT_PORT=80

#run-time environment variable => set using --env KEY=value
ENV PORT ${DEFAULT_PORT}

EXPOSE $PORT

CMD [ "npm", "start" ]