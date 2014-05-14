# Image: glerchundi/base-with-oracle-java7:12.04
FROM glerchundi/base:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

# Install Oracle Java 7
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update -y
RUN echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get install -y oracle-java7-installer

# Clean-up, as always
RUN apt-get clean && rm -rf /var/cache/apt/*
