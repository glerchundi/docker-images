# Image: glerchundi/mono-xsp4-app:3.4
FROM saltosystems/base:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

RUN wget -q http://download.opensuse.org/repositories/home:tpokorra:mono/xUbuntu_12.04/Release.key -O- | apt-key add - && \
    sh -c "echo 'deb http://download.opensuse.org/repositories/home:/tpokorra:/mono/xUbuntu_12.04/ /' >> /etc/apt/sources.list.d/mono-opt.list" && \
    apt-get -q update && \
    apt-get -y -q install mono-opt mono-xsp-opt

ADD assets/ /app/
RUN chmod 755 /app/init /app/setup/install
RUN /app/setup/install

EXPOSE 22
EXPOSE 8080

ENTRYPOINT ["/app/init"]
CMD ["app:start"]