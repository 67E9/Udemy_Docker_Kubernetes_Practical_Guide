FROM node:14-alpine

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

CMD ["node", "app.js"]

#docker run -d -p 80:80 --name docker-dep-test --rm docker-deployment-node-img