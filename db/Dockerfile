FROM dockerfile/mariadb
MAINTAINER Daniel Guerra

# To update, check http://guac-dev.org/releases
ENV GUACAMOLE_VERSION         0.9.9
ENV GUACAMOLE_WAR_SHA1        0ba2ff114ac4221794b148ab0e83370dbc6259c5
ENV GUACAMOLE_AUTH_JDBC_SHA1 a09558e46329e3f09ca30783d2e3008c11aae43b


COPY create-user.sql /schema/000-create-user.sql

# Fetch and install Guacamole JDBC auth extension.
RUN echo $GUACAMOLE_AUTH_JDBC_SHA1  guacamole-auth-jdbc.tar.gz > guacamole-auth-jdbc.tar.gz.sha1 && \
    curl -L -o guacamole-auth-jdbc.tar.gz http://sourceforge.net/projects/guacamole/files/current/extensions/guacamole-auth-jdbc-${GUACAMOLE_VERSION}.tar.gz/download && \
    sha1sum -c --quiet guacamole-auth-jdbc.tar.gz.sha1 && \
    tar xzf guacamole-auth-jdbc.tar.gz && \
    mv guacamole-auth-jdbc-${GUACAMOLE_VERSION}/mysql/*.jar /guacamole/classpath && \
    rm -rf guacamole-auth-jdbc*

# Setup database when container is started
RUN \
  echo "set -m" > /run.sh && \
  echo "mysqld_safe &" >> /run.sh && \
  echo "mysqladmin --silent --wait=30 ping || exit 1" >> /run.sh && \
  echo "mysqladmin create guacamole" >> /run.sh && \
  echo "cat /schema/*.sql | mysql guacamole" >> /run.sh && \
  echo "fg" >> /run.sh

CMD ["bash", "/run.sh"]
