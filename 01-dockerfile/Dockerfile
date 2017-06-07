FROM ubuntu
MAINTAINER yoheiMune
RUN apt-get update && \
	apt-get install -y nginx
ADD index.html /var/www/html
ENTRYPOINT /usr/sbin/nginx -g 'daemon off;' -c /etc/nginx/nginx.conf