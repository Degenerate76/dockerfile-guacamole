FROM alpine:edge
MAINTAINER Daniel Guerra

# To update, check http://guac-dev.org/releases
ENV GUACAMOLE_VERSION 0.9.9
ENV GUACAMOLE_SERVER_SHA1 a1ab3bf1e39291e318182b85055587fd98b39de1

COPY .abuild/-57cfc5fa.rsa.pub /etc/apk/keys/
COPY *.apk /tmp/
RUN apk add --update --no-cache make  automake autoconf gcc libtool curl libevent-dev libssl1.0 musl musl-dev libgcc cairo-dev libpng-dev libvncserver-dev openssl-dev libogg-dev libvorbis-dev libwebp-dev
RUN apk add /tmp/freerdp-1.2.0-r1.apk /tmp/freerdp-libs-1.2.0-r1.apk /tmp/freerdp-plugins-1.2.0-r1.apk /tmp/freerdp-dev-1.2.0-r1.apk
RUN apk add --update /tmp/ossp-uuid-1.6.2-r0.apk /tmp/ossp-uuid-dev-1.6.2-r0.apk
RUN echo $GUACAMOLE_SERVER_SHA1  guacamole-server.tar.gz > guacamole-server.tar.gz.sha1 && \
    curl -L -o guacamole-server.tar.gz http://sourceforge.net/projects/guacamole/files/current/source/guacamole-server-${GUACAMOLE_VERSION}.tar.gz/download && \
    sha1sum guacamole-server.tar.gz.sha1 | cut -d " " -f 1 && \
    tar xzf guacamole-server.tar.gz && \
    rm guacamole-server.tar.gz*
RUN cd guacamole-server-${GUACAMOLE_VERSION} && \
    ./configure && \
    make install && \
    ldconfig
# TODO: remove build tools and libs after installation


ENV GUACAMOLE_HOME /guacd
RUN mkdir -p /guacd
WORKDIR /guacd
EXPOSE 4822

CMD ["guacd", "-f", "-b", "0.0.0.0", "-l", "4822"]
