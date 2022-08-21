FROM node:13-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

WORKDIR /home/app
COPY ./app/package.json .
RUN npm install

COPY ./app .
# no need for /home/app/server.js because of WORKDIR
CMD ["node", "server.js"]
