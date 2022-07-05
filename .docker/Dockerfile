# Instalar ubuntu 20.04
FROM ubuntu:20.04

ARG php_version
ARG domain_host
ARG path_www

RUN DEBIAN_FRONTEND="noninteractive"
ENV CONTAINER_TIMEZONE="America/Sao_Paulo"
RUN ln -snf /usr/share/zoneinfo/$CONTAINER_TIMEZONE /etc/localtime && echo $CONTAINER_TIMEZONE > /etc/timezone

RUN apt-get update
RUN apt-get install -y nano

# Instalar Apache
RUN apt-get -y install apache2

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_RUN_DIR ${path_www}

# Instalar PHP
RUN apt-get -y install software-properties-common
RUN add-apt-repository ppa:ondrej/php

RUN apt-get -y install libapache2-mod-php${php_version}
RUN apt-get -y install php${php_version}-dev
RUN apt-get -y install php${php_version}-cli
RUN apt-get -y install php${php_version}-curl
RUN apt-get -y install php${php_version}-common
RUN apt-get -y install php${php_version}-calendar
RUN apt-get -y install php${php_version}-mbstring
RUN apt-get -y install php${php_version}-bcmath
RUN apt-get -y install php${php_version}-gd
RUN apt-get -y install php${php_version}-mysql
RUN apt-get -y install php${php_version}-json
RUN apt-get -y install php${php_version}-ldap
RUN apt-get -y install php${php_version}-pgsql
RUN apt-get -y install php${php_version}-tidy
RUN apt-get -y install php${php_version}-intl
RUN apt-get -y install php${php_version}-xml
RUN apt-get -y install php${php_version}-soap
RUN apt-get -y install php${php_version}-pdo
RUN apt-get -y install php${php_version}-zip
RUN apt-get -y install php${php_version}-xdebug
RUN apt-get -y install php-pear

# Habilitando o XDebug
RUN phpenmod xdebug

# Instalando Swoole
RUN pecl install openswoole

# Limpe os arquivos de configuração
RUN apt-get -y autoremove
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*
RUN rm -f ${path_www}/index.html

# Habilitando módulos apache2
RUN a2enmod rewrite
RUN a2enmod ssl
RUN a2enmod headers
RUN a2enmod expires
RUN a2enmod ext_filter
RUN a2enmod lbmethod_byrequests
RUN a2enmod php${php_version}

# Substituir a configuração padrão do apache e do php
RUN echo "ServerName 127.0.0.1" >> /etc/apache2/apache2.conf
RUN sed -i '/<Directory \/var\/www\/>/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf

# Habilitar SSL
RUN mkdir -p /etc/apache2/ssl

# Permissões
RUN chown -R root:www-data ${path_www}
RUN chmod u+rwx,g+rx,o+rx ${path_www}
RUN find ${path_www} -type d -exec chmod u+rwx,g+rx,o+rx {} +
RUN find ${path_www} -type f -exec chmod u+rw,g+rw,o+r {} +

RUN service apache2 restart

EXPOSE 80
EXPOSE 443

ENTRYPOINT ["apache2ctl", "-D", "FOREGROUND"]
