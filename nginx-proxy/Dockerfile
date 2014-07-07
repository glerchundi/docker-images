# Image: glerchundi/nginx-proxy:1.6
FROM saltosystems/base:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

RUN echo "deb http://nginx.org/packages/ubuntu/ precise nginx" > /etc/apt/sources.list.d/nginx.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABF5BD827BD9BF62 && \
    apt-get update && \
    apt-get install -y nginx

ADD assets/ /app/
RUN chmod 755 /app/init /app/setup/install
RUN /app/setup/install

EXPOSE 22
EXPOSE 80

#VOLUME ["/etc/postgresql", /var/lib/postgresql", "/var/log/postgresql"]

ENTRYPOINT ["/app/init"]
CMD ["app:start"]
