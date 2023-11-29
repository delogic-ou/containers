ARG NGINX_VERSION=${NGINX_VERSION:-1.24.0}

FROM nginx:$NGINX_VERSION as builder
ARG GEOIP2_VERSION=${GEOIP2_VERSION:-3.4}

RUN apt-get update \
    && apt-get install -y \
    build-essential \
    libpcre++-dev \
    zlib1g-dev \
    libgeoip-dev \
    libmaxminddb-dev \
    wget \
    git

RUN cd /opt \
    && git clone --depth 1 -b $GEOIP2_VERSION https://github.com/leev/ngx_http_geoip2_module.git \
    && wget -O - http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz | tar zxfv - \
    && mv /opt/nginx-$NGINX_VERSION /opt/nginx \
    && cd /opt/nginx \
    && ./configure --with-compat --add-dynamic-module=/opt/ngx_http_geoip2_module --with-stream \
    && make modules


FROM nginx:$NGINX_VERSION

LABEL org.opencontainers.image.authors="sre@delogic.io" \
      org.opencontainers.image.description="NGINX Docker image with GeoIP2 modules enabled" \
      org.opencontainers.image.licenses="Apache-2.0" \
      org.opencontainers.image.ref.name="debian:bullseye" \
      org.opencontainers.image.source="https://github.com/delogic-io/containers" \
      org.opencontainers.image.title="nginx-geoip2" \
      org.opencontainers.image.vendor="Delogic.io"

COPY --from=builder /opt/nginx/objs/ngx_http_geoip2_module.so /usr/lib/nginx/modules
COPY --from=builder /opt/nginx/objs/ngx_stream_geoip2_module.so /usr/lib/nginx/modules

RUN apt-get update \
    && apt-get install -y --no-install-recommends --no-install-suggests libmaxminddb0 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && chmod -R 755 /usr/lib/nginx/modules/*.so \
    && sed -i '1iload_module modules\/ngx_http_geoip2_module.so;' /etc/nginx/nginx.conf \
    && sed -i '2iload_module modules\/ngx_stream_geoip2_module.so;' /etc/nginx/nginx.conf
