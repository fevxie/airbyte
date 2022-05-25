ARG JDK_VERSION=17.0.1
ARG JDK_IMAGE=openjdk:${JDK_VERSION}-slim
FROM ${JDK_IMAGE} AS scheduler

ARG VERSION=0.38.3-alpha

ENV APPLICATION airbyte-scheduler
ENV VERSION ${VERSION}

RUN sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list \
    && rm -Rf /var/lib/apt/lists/* \
    && apt-get update

WORKDIR /app

ADD bin/${APPLICATION}-${VERSION}.tar /app

# wait for upstream dependencies to become available before starting server
ENTRYPOINT ["/bin/bash", "-c", "${APPLICATION}-${VERSION}/bin/${APPLICATION}"]
