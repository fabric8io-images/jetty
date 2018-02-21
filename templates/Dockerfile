FROM {{= fp.config.version.from.image }}

MAINTAINER {{= fp.maintainer }}

EXPOSE 8080

ENV JETTY_VERSION {{= fp.config.version.version }}
ENV DEPLOY_DIR /deployments
ENV JAVA /usr/bin/java

USER root

{{= fp.block("agent-bond", "install", { dest: "/opt/run-java-options" }) }}

RUN curl {{= fp.config.version.downloadUrl }} -o /tmp/jetty.tar.gz \
 && cd /opt && tar zxvf /tmp/jetty.tar.gz \
 && ln -s /opt/jetty-distribution-${JETTY_VERSION} /opt/jetty \
 && chown -R jboss /opt/jetty /opt/jetty-distribution-${JETTY_VERSION} \
 && usermod -g root -G jboss jboss \
 && chmod -R "g+rwX" /opt/jetty /opt/jetty-distribution-${JETTY_VERSION} \
 && chown -R jboss:root /opt/jetty /opt/jetty-distribution-${JETTY_VERSION} \
 && rm /tmp/jetty.tar.gz

# Startup script & permissions for OpenShift
ADD deploy-and-run.sh /opt/jetty/bin/
ADD jetty-logging.xml /opt/jetty/etc/
RUN chmod a+x /opt/jetty/bin/deploy-and-run.sh

{{= fp.block("run-java-sh","copy",{dest: "/opt/"}) }}

ENV JETTY_HOME /opt/jetty
ENV PATH $PATH:$JETTY_HOME/bin

CMD /opt/jetty/bin/deploy-and-run.sh

USER jboss
