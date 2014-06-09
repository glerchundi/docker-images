# Image: glerchundi/postgresql:9.3
FROM glerchundi/base:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

ENV POSTGRESQL_VERSION 9.3

RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
    wget --quiet --no-check-certificate -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
    apt-get update && \
    apt-get install -y postgresql-$POSTGRESQL_VERSION postgresql-client-$POSTGRESQL_VERSION

ADD assets/ /app/
RUN chmod 755 /app/init /app/setup/install
RUN /app/setup/install

EXPOSE 22
EXPOSE 5432

VOLUME ["/etc/postgresql", /var/lib/postgresql", "/var/log/postgresql"]

ENTRYPOINT ["/app/init"]
CMD ["app:start"]
