FROM ubuntu:jammy

ARG DEBIAN_FRONTEND=noninteractive

RUN apt update -y;
RUN apt install apache2 -y
RUN apt install ufw -y
RUN ufw allow in "Apache"


# Install PHP 7.4

RUN apt -y install software-properties-common
RUN add-apt-repository ppa:ondrej/php
RUN apt-get update
RUN apt -y install php7.4
RUN apt-get install -y php7.4-cli php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath


# Install the composer
RUN apt -y install curl
RUN cd ~
RUN curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
RUN HASH=`curl -sS https://composer.github.io/installer.sig`
RUN echo $HASH
RUN php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer


# Enable mode rewrite
RUN a2enmod rewrite
COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf 
RUN service apache2 restart


# Install PHP MongoDB extension
RUN apt-get install php7.4-mongodb -y
#COPY ./docker-mongo-db-ca-certificate.crt /usr/share/docker-mongo-db-ca-certificate.crt


WORKDIR /var/www/html
EXPOSE 80
RUN service apache2 start

CMD ["/bin/bash","-c", "sleep 360m"]


