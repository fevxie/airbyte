FROM python:3.9.11-alpine3.15 as base

# build and load all requirements
FROM base as builder
WORKDIR /airbyte/integration_code

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories
# upgrade pip to the latest version
RUN apk --no-cache upgrade \
    && pip install --upgrade pip \
    && apk --no-cache add tzdata build-base

COPY setup.py ./
# install necessary packages to a temporary folder
RUN python setup.py egg_info
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --prefix=/install -r *.egg-info/requires.txt

# build a clean environment
FROM base
WORKDIR /airbyte/source_acceptance_test

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories

# copy all loaded and built libraries to a pure basic image
COPY --from=builder /install /usr/local
# add default timezone settings
COPY --from=builder /usr/share/zoneinfo/Etc/UTC /etc/localtime
RUN echo "Etc/UTC" > /etc/timezone
# Bash is installed for more convenient debugging.
RUN apk --no-cache add bash

ENV ACCEPTANCE_TEST_DOCKER_CONTAINER 1

# copy payload code only
COPY pytest.ini setup.py ./
COPY source_acceptance_test ./source_acceptance_test
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple .

LABEL io.airbyte.version=0.1.51
LABEL io.airbyte.name=airbyte/source-acceptance-test

ENTRYPOINT ["python", "-m", "pytest", "-p", "source_acceptance_test.plugin", "-r", "fEsx"]
