FROM python:3.9-slim

RUN sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list

# Bash is installed for more convenient debugging.
RUN apt-get update && apt-get install -y bash && rm -rf /var/lib/apt/lists/*

ENV AIRBYTE_ENTRYPOINT "python /airbyte/integration_code/main.py"

WORKDIR /airbyte/integration_code
COPY source_gitlab ./source_gitlab
COPY main.py ./
COPY setup.py ./
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple .

ENTRYPOINT ["python", "/airbyte/integration_code/main.py"]

LABEL io.airbyte.version=0.1.5
LABEL io.airbyte.name=airbyte/source-gitlab
