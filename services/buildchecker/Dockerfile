# syntax=docker/dockerfile:1
ARG BASE_IMAGE=python:3.13-slim@sha256:f82c96458eedc847b233e582eb31336f4954b39cae020b6dcf5b3ed0e5cbcd74
FROM ${BASE_IMAGE}

ENV LC_ALL=C.UTF-8 \
    TZ=UTC \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# 【修改1】把 Debian 官方源替换为华为云源，加速 apt-get
RUN sed -i 's/deb.debian.org/mirrors.huaweicloud.com/g' /etc/apt/sources.list.d/debian.sources || \
    sed -i 's/deb.debian.org/mirrors.huaweicloud.com/g' /etc/apt/sources.list

RUN apt-get update \
 && apt-get install -y --no-install-recommends gcc make libc6-dev strace git \
 && rm -rf /var/lib/apt/lists/* \
 && dpkg-query -W -f='${Package}=${Version}\n' gcc make libc6-dev strace git > /opt/toolchain.lock \
 && echo "python3=$(python3 -c 'import platform; print(platform.python_version())')" >> /opt/toolchain.lock \
 && echo "arch=$(dpkg --print-architecture)" >> /opt/toolchain.lock

COPY requirements-dev.lock /tmp/requirements-dev.lock

# 【修改2】给 pip 指定清华源，加速 Python 依赖下载
RUN pip install --no-cache-dir --require-hashes -r /tmp/requirements-dev.lock -i https://pypi.tuna.tsinghua.edu.cn/simple \
 && pip freeze | sed 's/^/pip:/' >> /opt/toolchain.lock

RUN useradd --create-home --uid 10001 app && install -d -o app -g app /app
WORKDIR /app
COPY --chown=app:app services/buildchecker/ /app/
COPY --chown=app:app fixtures/ /app/fixtures/
USER app

CMD ["python3", "-m", "buildchecker", "--help"]
