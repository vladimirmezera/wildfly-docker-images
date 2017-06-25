FROM centos:centos7
MAINTAINER vladimir.mezera@gmail.com

LABEL Vendor="Vladimir Mezera"
LABEL Version=10.1.0.Final

ENV OPT /opt

# Update base image
RUN yum install epel-release -y
RUN yum install dnf -y
RUN dnf update -y && dnf clean all

RUN dnf -y install tar java-1.8.0-openjdk-devel saxon \ 
    augeas bsdtar shadow-utils && yum clean all
ENV WILDFLY_VERSION 10.1.0.Final

RUN groupadd -r wildfly -g 433 && useradd -u 431 -r -g wildfly -d /opt/wildfly -s /sbin/nologin -c "WildFly user" wildfly

RUN mkdir "/opt/wildfly-$WILDFLY_VERSION"

WORKDIR "${OPT}"
RUN curl "http://download.jboss.org/wildfly/$WILDFLY_VERSION/wildfly-$WILDFLY_VERSION.tar.gz" | tar zx && chown -R wildfly:wildfly "/opt/wildfly-$WILDFLY_VERSION"

RUN ln -s "/opt/wildfly-$WILDFLY_VERSION /opt/wildfly" && chown -R wildfly:wildfly /opt/wildfly
ENV JBOSS_HOME /opt/wildfly
EXPOSE 8080 9990
USER wildfly


# Set the default command to run on boot
# This will boot WildFly in the standalone mode and bind to all interface
CMD ["/opt/wildfly/bin/standalone.sh", "-c", "standalone.xml", "-b", "0.0.0.0", "-bmanagement", "0.0.0.0"]


