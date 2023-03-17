FROM node

WORKDIR /app

COPY package.json .

RUN npm install

ENV MONGO_USR admin
ENV MONGO_PW password

COPY . .

EXPOSE 80

CMD ["npm", "start"]