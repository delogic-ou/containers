FROM alpine:3

LABEL org.opencontainers.image.authors="sre@delogic.io" \
  org.opencontainers.image.description="Docker image with popular networking tools" \
  org.opencontainers.image.licenses="Apache-2.0" \
  org.opencontainers.image.ref.name="alpine:3" \
  org.opencontainers.image.source="https://github.com/delogic-io/containers" \
  org.opencontainers.image.title="net-tools" \
  org.opencontainers.image.vendor="Delogic.io"

RUN apk add --no-cache \
  bash \
  curl \
  tcpdump \
  traceroute \
  mtr \
  iproute2 \
  nmap \
  bind-tools \
  net-tools \
  inetutils-telnet
  
CMD ["/bin/bash"]
