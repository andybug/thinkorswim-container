FROM azul/zulu-openjdk:11

ARG UID 1000
ARG GID 1000

ENV DEBIAN_FRONTEND noninteractive

RUN addgroup --gid $GID thinkorswim && \
    adduser --uid $UID --gid $GID thinkorswim && \
    apt-get update && \
    apt-get -y install gconf2 tzdata
