FROM ubuntu:24.04

ARG SLP_VERSION=0.2.1
ARG SLP_SHA256=be4e57a5f55ba83136fb812e6a0c2261b8dccd3d228af2d0907085b7f5b06c5c
ARG ALP_VERSION=1.0.21
ARG ALP_SHA256=890628b2fe5307b637e4f875fd854a737f787d9ebc73d5f36554cfeaa7acd00d
ARG PPROTEIN_VERSION=1.2.4
ARG PPROTEIN_SHA256=04d57cf5177ae828121989be0be7bb49d4a8b2a7b907b9502e66a7807d325a01

LABEL org.opencontainers.image.source="https://github.com/mo124121/isucon-o11y"

RUN apt-get update && \
    apt-get install --no-install-recommends --yes \
      ca-certificates \
      graphviz \
      gv \
      wget && \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    cd /tmp; \
    wget --output-document=slp.tar.gz \
      "https://github.com/tkuchiki/slp/releases/download/v${SLP_VERSION}/slp_linux_amd64.tar.gz"; \
    echo "${SLP_SHA256}  slp.tar.gz" | sha256sum --check --strict; \
    tar --extract --gzip --file=slp.tar.gz slp; \
    install --mode=0755 slp /usr/local/bin/slp; \
    rm -f slp slp.tar.gz

RUN set -eux; \
    cd /tmp; \
    wget --output-document=alp.tar.gz \
      "https://github.com/tkuchiki/alp/releases/download/v${ALP_VERSION}/alp_linux_amd64.tar.gz"; \
    echo "${ALP_SHA256}  alp.tar.gz" | sha256sum --check --strict; \
    tar --extract --gzip --file=alp.tar.gz alp; \
    install --mode=0755 alp /usr/local/bin/alp; \
    rm -f alp alp.tar.gz

RUN set -eux; \
    cd /tmp; \
    wget --output-document=pprotein.tar.gz \
      "https://github.com/kaz/pprotein/releases/download/v${PPROTEIN_VERSION}/pprotein_${PPROTEIN_VERSION}_linux_amd64.tar.gz"; \
    echo "${PPROTEIN_SHA256}  pprotein.tar.gz" | sha256sum --check --strict; \
    tar --extract --gzip --file=pprotein.tar.gz pprotein; \
    install --mode=0755 pprotein /usr/local/bin/pprotein; \
    rm -f pprotein pprotein.tar.gz

WORKDIR /data

EXPOSE 9000

ENTRYPOINT [ "/usr/local/bin/pprotein" ]
