# Image: glerchundi/atlassian-bamboo-server:5.5.1
FROM glerchundi/base-with-oracle-java7:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

#
# Install Bamboo
#

# Environment
ENV BAMBOO_HOME /home/bamboo
ENV BAMBOO_VERSION 5.5.1
ENV BAMBOO_URL http://downloads.atlassian.com/software/bamboo/downloads/atlassian-bamboo-$BAMBOO_VERSION.tar.gz 

# VCS tools
RUN apt-get update && \
    apt-get install -y git subversion

# Bamboo itself
RUN wget -P /opt $BAMBOO_URL
RUN tar xvzf /opt/atlassian-bamboo-$BAMBOO_VERSION.tar.gz -C /opt
RUN rm /opt/atlassian-bamboo-$BAMBOO_VERSION.tar.gz
RUN ln -s /opt/atlassian-bamboo-$BAMBOO_VERSION /opt/atlassian-bamboo

#
# Install Docker
#

# Install requirements
RUN apt-get update && \
    apt-get install -y iptables ca-certificates lxc

ADD assets/ /app/
RUN chmod 755 /app/init /app/setup/install
RUN /app/setup/install

VOLUME [ "/home/bamboo", "/var/lib/docker" ]

EXPOSE 8085
EXPOSE 54663

ENTRYPOINT ["/app/init"]
CMD ["app:start"]
