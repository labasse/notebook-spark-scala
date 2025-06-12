# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.236.0/containers/java/.devcontainer/base.Dockerfile

# [Choice] Java version (use -bullseye variants on local arm64/Apple Silicon): 11, 17, 11-bullseye, 17-bullseye, 11-buster, 17-buster
ARG VARIANT="21-bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/java:${VARIANT}

ARG SPARK_VERSION=3.5.6
ARG SETUP_DIR=~vscode/spark

ENV SPARK_DIST="spark-${SPARK_VERSION}-bin-hadoop3"
ENV VENV_DIR="${SETUP_DIR}/.venv"

COPY --chmod=755 spark /etc/init.d/spark

RUN apt update -y && \
    apt install -y --no-install-recommends \
    python3-pip python3-venv && \
    wget https://dlcdn.apache.org/spark/spark-${SPARK_VERSION}/${SPARK_DIST}.tgz && \
    tar -xzf ${SPARK_DIST}.tgz && \
    mv ${SPARK_DIST} ${SETUP_DIR} && \
    rm ${SPARK_DIST}.tgz && \
    python3 -m venv ${VENV_DIR} && \
    ${VENV_DIR}/bin/pip install --upgrade pip && \
    ${VENV_DIR}/bin/pip install toree && \
    ${VENV_DIR}/bin/pip install jupyter && \
    ${VENV_DIR}/bin/jupyter toree install --spark_home=${SETUP_DIR} && \
    chmod +x /etc/init.d/spark && \
    update-rc.d spark defaults && \
    update-rc.d spark enable && \
    service spark start 
