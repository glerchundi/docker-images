# Image: glerchundi/atlassian-jira:6.2
FROM glerchundi/base-with-oracle-java7:12.04
MAINTAINER Gorka Lerchundi Osa <glertxundi@gmail.com>

# Environment
ENV JIRA_HOME /home/jira
ENV JIRA_VERSION 6.2.7
ENV JIRA_URL http://downloads.atlassian.com/software/jira/downloads/atlassian-jira-$JIRA_VERSION.tar.gz

# JIRA
RUN wget -P /opt $JIRA_URL
RUN tar xvzf /opt/atlassian-jira-$JIRA_VERSION.tar.gz -C /opt
RUN rm /opt/atlassian-jira-$JIRA_VERSION.tar.gz
RUN ln -s /opt/atlassian-jira-$JIRA_VERSION-standalone /opt/atlassian-jira

ADD assets/ /app/
RUN chmod 755 /app/init /app/setup/install
RUN /app/setup/install

VOLUME ["/home/jira"]

EXPOSE 8080

ENTRYPOINT ["/app/init"]
CMD ["app:start"]
