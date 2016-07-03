FROM jboss/base-jdk:8

MAINTAINER rhuss@redhat.com

EXPOSE 8080

ENV JETTY_VERSION 7.6.19.v20160209
ENV DEPLOY_DIR /deployments
ENV JAVA /usr/bin/java

USER root

# Agent bond
# Agent bond including Jolokia and jmx_exporter
ADD agent-bond-opts /opt/run-java-options
RUN mkdir -p /opt/agent-bond \
 && curl http://central.maven.org/maven2/io/fabric8/agent-bond-agent/0.1.3/agent-bond-agent-0.1.3.jar \
          -o /opt/agent-bond/agent-bond.jar \
 && chmod 444 /opt/agent-bond/agent-bond.jar \
 && chmod 755 /opt/run-java-options
ADD jmx_exporter_config.yml /opt/agent-bond/
EXPOSE 8778 9779



RUN curl http://repo1.maven.org/maven2/org/eclipse/jetty/jetty-distribution/${JETTY_VERSION}/jetty-distribution-${JETTY_VERSION}.tar.gz -o /tmp/jetty.tar.gz \
 && cd /opt && tar zxvf /tmp/jetty.tar.gz \
 && ln -s /opt/jetty-distribution-${JETTY_VERSION} /opt/jetty \
 && chown -R jboss /opt/jetty /opt/jetty-distribution-${JETTY_VERSION} \
 && rm /tmp/jetty.tar.gz

# Startup script
ADD deploy-and-run.sh /opt/jetty/bin/
ADD jetty-logging.xml /opt/jetty/etc/
RUN chmod a+x /opt/jetty/bin/deploy-and-run.sh

# java-container-options
# Add java-container-options to /opt/jetty/bin and make it executable
COPY java-container-options /opt/jetty/bin/java-container-options
RUN chmod 755 /opt/jetty/bin/java-container-options


ENV JETTY_HOME /opt/jetty
ENV PATH $PATH:$JETTY_HOME/bin

CMD /opt/jetty/bin/deploy-and-run.sh

USER jboss
