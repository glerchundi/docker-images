# Image: glerchundi/mariadb:10.0
FROM glerchundi/base:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

ENV MARIADB_VERSION 10.0

RUN apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db && \
	add-apt-repository "deb http://mirror.timeweb.ru/mariadb/repo/$MARIADB_VERSION/ubuntu precise main" && \
	apt-get update && \
	apt-get install -y mariadb-server

ADD assets/ /app/
RUN chmod 755 /app/init /app/setup/install
RUN /app/setup/install

EXPOSE 22
EXPOSE 3306

VOLUME ["/etc/mysql", /var/lib/mysql", "/var/log/mysql"]

ENTRYPOINT ["/app/init"]
CMD ["app:start"]