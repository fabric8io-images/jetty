FROM {{= fp.config.version.from.image }}

MAINTAINER {{= fp.maintainer }}

EXPOSE 8080

ENV JETTY_VERSION {{= fp.config.version.version }}
ENV DEPLOY_DIR /deployments
ENV JAVA /usr/bin/java

USER root

# Agent bond
{{= fp.block("agent-bond", "install", { dest: "/opt/run-java-options" }) }}

RUN curl {{= fp.config.version.downloadUrl }} -o /tmp/jetty.tar.gz \
 && cd /opt && tar zxvf /tmp/jetty.tar.gz \
 && ln -s /opt/jetty-distribution-${JETTY_VERSION} /opt/jetty \
 && chown -R jboss /opt/jetty /opt/jetty-distribution-${JETTY_VERSION} \
 && rm /tmp/jetty.tar.gz

# Startup script
ADD deploy-and-run.sh /opt/jetty/bin/
ADD jetty-logging.xml /opt/jetty/etc/
RUN chmod a+x /opt/jetty/bin/deploy-and-run.sh

# java-container-options for container specific java startup options
{{= fp.block("run-java-sh","copy-java-container-options",{dest: "/opt/jetty/bin"}) }}

ENV JETTY_HOME /opt/jetty
ENV PATH $PATH:$JETTY_HOME/bin

CMD /opt/jetty/bin/deploy-and-run.sh

USER jboss
