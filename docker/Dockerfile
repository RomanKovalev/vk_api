FROM node:11.13.0-alpine

RUN mkdir -p /usr/src/webapp
WORKDIR /usr/src/webapp

RUN apk update && apk upgrade
RUN apk add git

RUN git clone https://github.com/RomanKovalev/vk_api.git .
RUN npm install

EXPOSE 8080

# ENV NUXT_HOST=0.0.0.0
# ENV NUXT_PORT=3000

CMD [ "yarn", "serve" ]