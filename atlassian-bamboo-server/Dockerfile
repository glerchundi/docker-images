# Image: glerchundi/atlassian-bamboo-server:5.4.2
FROM glerchundi/base-with-oracle-java7:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

#
# Install Bamboo
#

# Environment
ENV BAMBOO_HOME /home/bamboo
ENV BAMBOO_VERSION 5.4.2
ENV BAMBOO_URL http://downloads.atlassian.com/software/bamboo/downloads/atlassian-bamboo-$BAMBOO_VERSION.tar.gz 

# VCS tools
RUN apt-get install -y git subversion

# Bamboo itself
RUN wget -P /opt $BAMBOO_URL
RUN tar xvzf /opt/atlassian-bamboo-$BAMBOO_VERSION.tar.gz -C /opt
RUN rm /opt/atlassian-bamboo-$BAMBOO_VERSION.tar.gz
RUN ln -s /opt/atlassian-bamboo-$BAMBOO_VERSION /opt/atlassian-bamboo

# Uncomment to increase Tomcat's maximum heap allocation
# export JAVA_OPTS=-Xmx512M $JAVA_OPTS

#
# Install Docker
#

# Install requirements
RUN echo deb http://archive.ubuntu.com/ubuntu precise universe > /etc/apt/sources.list.d/universe.list
RUN apt-get update -y && apt-get install -y iptables ca-certificates lxc
RUN apt-get install -y iptables ca-certificates lxc

# Add docker requirements
ADD https://get.docker.io/builds/Linux/x86_64/docker-latest /usr/local/bin/docker
ADD assets/wrapdocker /usr/local/bin/wrapdocker
RUN chmod +x /usr/local/bin/docker /usr/local/bin/wrapdocker

VOLUME /var/lib/docker

# First run docker wrapper
CMD wrapdocker

# Add custom scripts
ADD assets/build-and-push /usr/local/bin/build-and-push
RUN chmod +x /usr/local/bin/build-and-push

# Expose web and agent ports
EXPOSE 8085
EXPOSE 54663

# ..then the entry point
ENTRYPOINT ["/opt/atlassian-bamboo/bin/catalina.sh", "run"]
