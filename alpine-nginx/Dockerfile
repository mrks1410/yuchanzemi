FROM alpine:latest

ARG nginx_dir="/etc/nginx/"

RUN apk add openrc && apk add nginx

RUN sed -i 's/#rc_sys=""/rc_sys="lxc"/g' /etc/rc.conf && \
	echo 'rc_provide="loopback net"' >> /etc/rc.conf && \
	sed -i 's/^#\(rc_logger="YES"\)$/\1/' /etc/rc.conf && \
	sed -i '/tty/d' /etc/inittab && \
	sed -i 's/hostname $opts/# hostname $opts/g' /etc/init.d/hostname && \
	sed -i 's/mount -t tmpfs/# mount -t tmpfs/g' /lib/rc/sh/init.sh && \
	sed -i 's/cgroup_add_service /# cgroup_add_service /g' /lib/rc/sh/openrc-run.sh && \
	rm -rf /var/cache/apk/*
RUN mv ${nginx_dir}nginx.conf ${nginx_dir}nginx.conf.org
RUN mkdir /www

ADD index.html /www
ADD nginx.conf ${nginx_dir}

RUN adduser -D -g 'www' www && \
	chown -R www:www /var/lib/nginx && \
	chown -R www:www /www

EXPOSE 80

CMD ["/sbin/init"]
