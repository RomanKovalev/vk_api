FROM node:11.13.0-alpine

RUN mkdir -p /usr/src/webapp
WORKDIR /usr/src/webapp

RUN apk update && apk upgrade
RUN apk add git

RUN git clone https://github.com/RomanKovalev/vk_api.git .
RUN npm install

EXPOSE 8080

CMD [ "yarn", "serve" ]

# docker build -t vk_cont .
# docker run -dit -p 8080:8080 vk_cont
