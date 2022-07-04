# Instalar ubuntu 20.04
FROM ubuntu:20.04

#ARG UID=root
#ARG GID=root
#ARG USER

RUN DEBIAN_FRONTEND="noninteractive"
ENV CONTAINER_TIMEZONE="America/Sao_Paulo"
RUN ln -snf /usr/share/zoneinfo/$CONTAINER_TIMEZONE /etc/localtime && echo $CONTAINER_TIMEZONE > /etc/timezone

RUN apt-get update
#RUN apt-get install sudo

# Instalar Apache
RUN apt-get -y install apache2

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_RUN_DIR /var/www/html

# Instalar PHP
RUN apt-get -y install software-properties-common
RUN add-apt-repository ppa:ondrej/php

RUN apt-get -y install php7.4-dev
RUN apt-get -y install php7.4-cli
RUN apt-get -y install php7.4-curl
RUN apt-get -y install php7.4-common
RUN apt-get -y install php7.4-calendar
RUN apt-get -y install php7.4-mbstring
RUN apt-get -y install php7.4-bcmath
RUN apt-get -y install php7.4-gd
RUN apt-get -y install php7.4-mysql
RUN apt-get -y install php7.4-json
RUN apt-get -y install php7.4-ldap
RUN apt-get -y install php7.4-pgsql
RUN apt-get -y install php7.4-tidy
RUN apt-get -y install php7.4-intl
RUN apt-get -y install php7.4-xml
RUN apt-get -y install php7.4-soap
RUN apt-get -y install php7.4-pdo
RUN apt-get -y install php7.4-zip
RUN apt-get -y install php7.4-xdebug
RUN apt-get -y install php-pear

RUN phpenmod xdebug

# Habilitando o XDebug
COPY 99-xdebug.ini "/etc/php/7.4/mods-available/xdebug.ini"

# Instalando Swoole
RUN pecl install openswoole

# Limpe os arquivos de configuração
RUN apt-get -y autoremove
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*
RUN rm -f /var/www/html/index.html

# Habilitando módulos apache2
RUN a2enmod rewrite
RUN a2enmod ssl
RUN a2enmod headers
RUN a2enmod expires
RUN a2enmod ext_filter
RUN a2enmod lbmethod_byrequests

# Gerar SSL
RUN apt-get update
RUN mkdir /etc/apache2/ssl/
RUN openssl req  -subj "/C=BR/CN=api.mastertax.local" -x509 -nodes -days 365  -newkey rsa:2048 -keyout /etc/apache2/ssl/api.mastertax.local.key -out /etc/apache2/ssl/api.mastertax.local.crt

# Substituir a configuração padrão do apache e do php
RUN echo "ServerName 127.0.0.1" >> /etc/apache2/apache2.conf
RUN sed -i '/<Directory \/var\/www\/>/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf

COPY 000-default.conf /etc/apache2/sites-available
COPY mpm_prefork.conf /etc/apache2/mods-available
COPY status.conf      /etc/apache2/mods-available
COPY api.mastertax.local.conf /etc/apache2/sites-available
COPY 99-local.ini     /etc/php/7.4/apache2/conf.d

# Habilita o virtual host
RUN a2ensite api.mastertax.local.conf

# Permissões
RUN chown -R root:www-data /var/www/html
RUN chmod u+rwx,g+rx,o+rx /var/www/html
RUN find /var/www/html -type d -exec chmod u+rwx,g+rx,o+rx {} +
RUN find /var/www/html -type f -exec chmod u+rw,g+rw,o+r {} +

WORKDIR /var/www/html

#RUN chown -R ${UID}:${GID} /var/www
#VOLUME /var/www

#RUN useradd -G www-data, root -u $uid -d /home/$user $user
# RUN sudo -S chmod 2775 /var/www
# RUN sudo -S chown .adm /var/www

#RUN service apache2 restart

#RUN . /etc/apache2/envvars

EXPOSE 80
EXPOSE 443
#USER ${UID}

#ENTRYPOINT ["apache2ctl", "-D", "FOREGROUND"]