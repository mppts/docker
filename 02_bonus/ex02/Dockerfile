FROM node

ENV NPM_CONFIG_LOGLEVEL warn

COPY . .

WORKDIR ./app

RUN npm install
EXPOSE 3000

ENTRYPOINT npm start


