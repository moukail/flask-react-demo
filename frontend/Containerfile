FROM docker.io/library/node:25.9-alpine3.23 AS builder

WORKDIR /var/www/app
COPY . .

RUN npm install && npm run build

#FROM docker.io/library/nginx:1.29-alpine3.23
FROM registry.access.redhat.com/ubi9/nginx-126:9.7

VOLUME /var/cache/nginx

COPY --from=builder --chown=1001:0 /var/www/app/dist /opt/app-root/src

EXPOSE 8080
STOPSIGNAL SIGTERM

USER 1001
CMD ["nginx", "-g", "daemon off;"]
