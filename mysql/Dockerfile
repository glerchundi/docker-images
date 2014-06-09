# Image: glerchundi/mysql:5.5
FROM glerchundi/base:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

RUN apt-get install -y mysql-server

ADD assets/ /app/
RUN chmod 755 /app/init /app/setup/install
RUN /app/setup/install

EXPOSE 22
EXPOSE 3306

VOLUME ["/etc/mysql", /var/lib/mysql", "/var/log/mysql"]

ENTRYPOINT ["/app/init"]
CMD ["app:start"]