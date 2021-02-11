# dev docker for all nodejs backend and frontend (ionic|angular|react|nestjs)
FROM node:14-alpine

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
    apk add python python-dev py2-pip autoconf automake g++ make --no-cache && \
    pip install py-bcrypt

# Hugely possible that this is not working anymore, as of typescript|ts-node 8
# RUN yarn global add typescript

# RUN yarn global add ts-node

RUN yarn global add @angular/cli

#RUN chown -R $(whoami) /usr/local/lib/node_modules && \
#    chown -R $(whoami) /usr/local/bin && \
#    chown -R $(whoami) /usr/local/share && \
#    chown -R $(whoami) ~/.npm && \

RUN yarn global add --unsafe-perm @ionic/cli native-run cordova cordova-res

RUN yarn global add create-react-app \
                    create-react-native-app \
                    react-native-cli

RUN yarn global add @vue/cli \
    && yarn global add @vue/cli-service-global \
    && yarn global add @vue/cli-init

RUN yarn global add @nestjs/cli

RUN mkdir /app

WORKDIR /app
