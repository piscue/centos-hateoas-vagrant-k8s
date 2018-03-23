FROM haproxy:1.8.4-alpine
MAINTAINER Victor Sanahuja piscue@gmail.com
COPY haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg
RUN adduser -D haproxy
EXPOSE 443
CMD ["haproxy", "-f /usr/local/etc/haproxy/haproxy.cfg"]
CMD ["sh"]
