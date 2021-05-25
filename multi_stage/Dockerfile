FROM node:13-slim as APP

COPY ./app /app

WORKDIR /usr/src/app

COPY ./app/package*.json ./

RUN npm install

COPY ./app .

FROM node:alpine

COPY --from=app /usr/src/app /usr/src/app

WORKDIR /usr/src/app

EXPOSE 3000

CMD ["node", "app.js"]
