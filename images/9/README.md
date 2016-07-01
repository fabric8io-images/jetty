## Jetty 9.3.10.v20160621

A simple docker build for installing a vanilla Jetty 9.3.10.v20160621 below */opt/jetty*. It comes out of the box and is intended for use in integration tests

During startup a directory specified by the environment variable `DEPLOY_DIR` (*/deployments* by default) is checked for .war files. If there are any, they are linked into the *webapps/* directory for automatic deployment. This plays nicely with the [docker-maven-plugin](https://github.com/fabric8io/docker-maven-plugin/) and its 'assembly' mode which can automatically create Docker data container with Maven artifacts exposed from a directory */deployments*.

### Agent Bond

For this image [Agent Bond](https://github.com/fabric8io/agent-bond) is enabled. Agent Bond exports metrics from [Jolokia](http://www.jolokia.org) and [jmx_exporter](https://github.com/prometheus/jmx_exporter).

Please refer to the base image's [documentation](https://github.com/fabric8io-images/java/tree/master/images/centos/openjdk8/jre) for available options to tune the Java JVM.

Features:

* Jetty Version: **9.3.10.v20160621**
* Java Base Image: **fabric8/java-centos-openjdk8-jre:1.1.4**
* Port: **8080**
* Command: `/opt/jetty/bin/deploy-and-run.sh` which links .war files from */maven* to */opt/jetty/webapps* and then calls `/opt/jetty/bin/jetty.sh run`
* Sets `-Djava.security.egd=file:/dev/./urandom` for faster startup times (though a bit less secure).
