FROM alpine:3.2
MAINTAINER Alexander Ivanovsky <sunx2ych@gmail.com>

ENV SELENIUM_DIR /selenium

RUN mkdir ${SELENIUM_DIR}
WORKDIR ${SELENIUM_DIR}

RUN apk update && \
    apk add openjdk7-jre-base && \
    rm -rf /var/cache/apk/*

ADD http://selenium-release.storage.googleapis.com/2.48/selenium-server-standalone-2.48.2.jar ${SELENIUM_DIR}