FROM nginx:alpine

COPY default.conf.template /etc/nginx/templates/default.conf.template
COPY index.html /usr/share/nginx/html/index.html

ENV BACKEND_HOST=bmi-service
ENV BACKEND_PORT=8000
