FROM --platform=$BUILDPLATFORM debian:bookworm-slim AS downloader

ARG TARGETARCH
ARG SLP_VERSION=0.2.1
ARG SLP_SHA256_AMD64=be4e57a5f55ba83136fb812e6a0c2261b8dccd3d228af2d0907085b7f5b06c5c
ARG SLP_SHA256_ARM64=0900b574a9857d3bbd1b060a8280b4c8c2f26b92fae28a215462333f7b8002d3
ARG ALP_VERSION=1.0.21
ARG ALP_SHA256_AMD64=890628b2fe5307b637e4f875fd854a737f787d9ebc73d5f36554cfeaa7acd00d
ARG ALP_SHA256_ARM64=ce3b82c0279f0439928c892dfb158de9f8c8eba5e4ea992f6a014b9e6c1e790c
ARG PPROTEIN_VERSION=1.2.4
ARG PPROTEIN_SHA256_AMD64=04d57cf5177ae828121989be0be7bb49d4a8b2a7b907b9502e66a7807d325a01
ARG PPROTEIN_SHA256_ARM64=b3a44e3c58a2c3c9b221635ef2d6590c5646ce4d96e20f55306d2b85bdb4fc2f

RUN apt-get update && \
    apt-get install --no-install-recommends --yes \
      ca-certificates \
      wget && \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    case "${TARGETARCH}" in \
      amd64) asset_arch=amd64; checksum="${SLP_SHA256_AMD64}" ;; \
      arm64) asset_arch=arm64; checksum="${SLP_SHA256_ARM64}" ;; \
      *) echo "unsupported architecture: ${TARGETARCH}" >&2; exit 1 ;; \
    esac; \
    mkdir -p /out /tmp/releases; \
    cd /tmp/releases; \
    wget --output-document=slp.tar.gz \
      "https://github.com/tkuchiki/slp/releases/download/v${SLP_VERSION}/slp_linux_${asset_arch}.tar.gz"; \
    echo "${checksum}  slp.tar.gz" | sha256sum --check --strict; \
    tar --extract --gzip --file=slp.tar.gz slp; \
    install --mode=0755 slp /out/slp; \
    rm -f slp slp.tar.gz

RUN set -eux; \
    case "${TARGETARCH}" in \
      amd64) asset_arch=amd64; checksum="${ALP_SHA256_AMD64}" ;; \
      arm64) asset_arch=arm64; checksum="${ALP_SHA256_ARM64}" ;; \
      *) echo "unsupported architecture: ${TARGETARCH}" >&2; exit 1 ;; \
    esac; \
    cd /tmp/releases; \
    wget --output-document=alp.tar.gz \
      "https://github.com/tkuchiki/alp/releases/download/v${ALP_VERSION}/alp_linux_${asset_arch}.tar.gz"; \
    echo "${checksum}  alp.tar.gz" | sha256sum --check --strict; \
    tar --extract --gzip --file=alp.tar.gz alp; \
    install --mode=0755 alp /out/alp; \
    rm -f alp alp.tar.gz

RUN set -eux; \
    case "${TARGETARCH}" in \
      amd64) asset_arch=amd64; checksum="${PPROTEIN_SHA256_AMD64}" ;; \
      arm64) asset_arch=arm64; checksum="${PPROTEIN_SHA256_ARM64}" ;; \
      *) echo "unsupported architecture: ${TARGETARCH}" >&2; exit 1 ;; \
    esac; \
    cd /tmp/releases; \
    wget --output-document=pprotein.tar.gz \
      "https://github.com/kaz/pprotein/releases/download/v${PPROTEIN_VERSION}/pprotein_${PPROTEIN_VERSION}_linux_${asset_arch}.tar.gz"; \
    echo "${checksum}  pprotein.tar.gz" | sha256sum --check --strict; \
    tar --extract --gzip --file=pprotein.tar.gz pprotein; \
    install --mode=0755 pprotein /out/pprotein; \
    rm -f pprotein pprotein.tar.gz

FROM debian:bookworm-slim

LABEL org.opencontainers.image.source="https://github.com/mo124121/isucon-o11y"

RUN apt-get update && \
    apt-get install --no-install-recommends --yes \
      ca-certificates \
      graphviz && \
    rm -rf /var/lib/apt/lists/*

COPY --from=downloader /out/slp /usr/local/bin/slp
COPY --from=downloader /out/alp /usr/local/bin/alp
COPY --from=downloader /out/pprotein /usr/local/bin/pprotein

WORKDIR /data

EXPOSE 9000

ENTRYPOINT [ "/usr/local/bin/pprotein" ]
