FROM ubuntu:18.04

# Get user id and group id from arguments.
ARG UID
ARG GID

# Timezone.
ENV TZ 'UTC'
ENV DEBIAN_FRONTEND=noninteractive
RUN echo $TZ > /etc/timezone && \
  apt-get update && apt-get install -y tzdata && \
  rm /etc/localtime && \
  ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
  dpkg-reconfigure -f noninteractive tzdata

# Install apache.
RUN apt-get update && apt-get install -y --no-install-recommends apache2 libapache2-mod-qos ca-certificates nano && \
  apt-get clean && rm -rf /var/cache/apt/* && rm -rf /var/lib/apt/lists/*

# Configuration.
RUN a2enmod rewrite proxy proxy_http proxy_fcgi headers proxy_wstunnel ssl && \
  echo 'ServerName localhost' >> /etc/apache2/apache2.conf && \  
  sed -i -e"s/^export\s*APACHE_RUN_USER=www-data/export APACHE_RUN_USER=serveruser/" /etc/apache2/envvars && \
  sed -i -e"s/^export\s*APACHE_RUN_GROUP=www-data/export APACHE_RUN_GROUP=serveruser/" /etc/apache2/envvars && \
  groupadd -r serveruser -g $GID && useradd -ms /bin/bash serveruser -u $UID -g $GID && \
  chmod -R 777 /var/log/apache2

# Add vhosts and enable sites.
COPY vhosts /etc/apache2/sites-available
RUN cd /etc/apache2/sites-available && a2ensite * && a2dissite default-ssl 000-default

RUN chown -R serveruser:serveruser /home/serveruser

EXPOSE 80 443

WORKDIR /home/serveruser

CMD ["/usr/sbin/apachectl", "-DFOREGROUND"]
