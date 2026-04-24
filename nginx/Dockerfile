FROM debian:bookworm-slim

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        nginx \
        libnginx-mod-http-modsecurity \
        modsecurity-crs \
        ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Bootstrap base ModSecurity config in DetectionOnly for safe rollout.
RUN cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf \
    && sed -i 's/^SecRuleEngine .*/SecRuleEngine DetectionOnly/' /etc/modsecurity/modsecurity.conf \
    && cp /usr/share/modsecurity-crs/crs-setup.conf.example /usr/share/modsecurity-crs/crs-setup.conf

COPY nginx/nginx.conf /etc/nginx/nginx.conf
COPY nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf
COPY nginx/modsec/main.conf /etc/nginx/modsec/main.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
