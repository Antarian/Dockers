# dev docker for all nodejs backend and frontend (ionic|angular|react|nestjs)
FROM node:lts-alpine

# node image is already adding user and group id 1000 named node
RUN if [ -z "`getent group 1000`" ]; then \
    addgroup -g 1000 -S username; \
    fi

RUN if [ -z "`getent passwd 1000`" ]; then \
    adduser -u 1000 -S username -G username; \
    fi

RUN npm install -g npm@latest

RUN apk add --no-cache --virtual .build-deps \
    ca-certificates \
    wget \
    tar && \
    cd /usr/local/bin && \
    wget https://yarnpkg.com/latest.tar.gz && \
    tar zvxf latest.tar.gz && \
    ln -s /usr/local/bin/dist/bin/yarn.js /usr/local/bin/yarn.js && \
    apk add git && \
    apk del .build-deps

RUN yarn add global node-gyp && \
#    apk add py3-setuptools py3-pip py3-bcrypt && \
    apk add --update musl-dev gcc libffi-dev && \
    apk add py3-setuptools py3-pip py3-bcrypt
#    pip install bcrypt
# apk add python python-dev py2-pip autoconf automake g++ make --no-cache && \
# pip install py-bcrypt

# Hugely possible that this is not working anymore, as of typescript|ts-node 8
# RUN yarn global add typescript
# RUN yarn global add ts-node

RUN yarn global add @angular/cli

RUN ng config --global cli.packageManager yarn

RUN yarn global add vue

RUN yarn global add create-react-app \
                    create-react-native-app \
                    react-native-cli \
                    serve

RUN yarn global add @nestjs/cli

RUN mkdir /app

WORKDIR /app
