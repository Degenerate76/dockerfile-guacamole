FROM tomcat:9.0-jre8-alpine
MAINTAINER Degenerate76

# To update, check http://guacamole.apache.org/releases and https://jdbc.postgresql.org/download.html
ENV GUACAMOLE_VERSION          1.1.0
ENV GUACAMOLE_WAR_SHA256       44e8c849d7183c91715666758ca66a58e7b5d2e9094e10680dcd609b39b92822
ENV GUACAMOLE_AUTH_JDBC_SHA256 ce65df072b488699ce1e862eb7b38a82a9497ef585df3b78312bde16df3a9543
ENV POSTGRES_CONNECTOR_VERSION 42.2.14
ENV POSTGRES_CONNECTOR_SHA256  48bbba05845b40bcce66ece3d7652153d27b5379d5ae90977b78eefd7c7a0287

RUN apk add --update curl
### Guacamole webapp
# Disable Tomcat's manager application.
RUN rm -rf webapps/*

# Fetch and install Guacamole war archive.
RUN echo $GUACAMOLE_WAR_SHA256  ROOT.war > webapps/ROOT.war.sha256 && \
    curl -L -o webapps/ROOT.war https://www.apache.org/dist/guacamole/${GUACAMOLE_VERSION}/binary/guacamole-${GUACAMOLE_VERSION}.war && \
    cd webapps && sha256sum ROOT.war.sha1 | cut -d " " -f 1

# Expose tomcat runtime options through the RUNTIME_OPTS environment variable.
#   Example to set the JVM's max heap size to 256MB use the flag
#   '-e RUNTIME_OPTS="-Xmx256m"' when starting a container.
RUN echo 'export CATALINA_OPTS="$RUNTIME_OPTS"' > bin/setenv.sh


### Guacamole Postgres auth extension
# Fetch and install Guacamole JDBC auth extension libs
RUN mkdir -p /guacamole/extensions && \
    mkdir -p /guacamole/lib && \
    mkdir -p /guacamole/bin
RUN echo $GUACAMOLE_AUTH_JDBC_SHA256  guacamole-auth-jdbc.tar.gz > guacamole-auth-jdbc.tar.gz.sha256 && \
    curl -L -o guacamole-auth-jdbc.tar.gz https://www.apache.org/dist/guacamole/${GUACAMOLE_VERSION}/binary/guacamole-auth-jdbc-${GUACAMOLE_VERSION}.tar.gz && \
    sha256sum guacamole-auth-jdbc.tar.gz.sha1 | cut -d " " -f 1 && \
    tar xzf guacamole-auth-jdbc.tar.gz && \
    mv guacamole-auth-jdbc-${GUACAMOLE_VERSION}/postgresql/*.jar /guacamole/extensions && \
    rm -rf guacamole-auth-jdbc*

# Fetch and install Postgres connector
RUN echo $POSTGRES_CONNECTOR_SHA256  postgres-connector.jar > postgres-connector.jar.sha256 && \
    curl -L -o postgres-connector.jar https://jdbc.postgresql.org/download/postgresql-${POSTGRES_CONNECTOR_VERSION}.jar && \
    sha256sum postgres-connector.jar.sha256 | cut -d " " -f 1 && \
    mv postgres-connector.jar /guacamole/lib


### Configuration
ENV GUACAMOLE_HOME /guacamole
COPY start.sh ${GUACAMOLE_HOME}/bin/
RUN chmod 544 ${GUACAMOLE_HOME}/bin/start.sh
EXPOSE 8080
CMD ["/guacamole/bin/start.sh"]
