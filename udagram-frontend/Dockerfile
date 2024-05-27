### Build
FROM node:16 AS node
## Create app directory
WORKDIR /usr/src/app
## Install app dependencies
## A wildcard is used to ensure both package.json AND package-lock.json are copied
RUN npm install -g @ionic/cli
COPY package*.json ./
RUN npm i -f
## Bundle app source
COPY . .
RUN ionic build
### Run 
FROM nginx:alpine
#COPY www /usr/share/nginx/html
COPY --from=ionic  /usr/src/app/www /usr/share/nginx/html