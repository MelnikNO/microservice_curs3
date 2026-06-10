FROM nginx:alpine

RUN apk add --no-cache curl

RUN mkdir -p /var/cache/nginx

COPY nginx/nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]