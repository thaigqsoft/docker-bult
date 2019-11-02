#php ใช้ version 7.3 สามารถเปลี่ยนได้ตามที่เราต้องการแต่ต้องเป็นตัว fpm นะครับ เพื่อที่จะให้กับ nginx
FROM php:7.3-fpm 

#Install คำสั่งสำหรับการลง package ที่ laravel required ไว้นะครับ
RUN apt-get update \
  && apt-get install -y \
  cron \
  libfreetype6-dev \
  libicu-dev \
  libjpeg62-turbo-dev \
  libmcrypt-dev \
  libxslt1-dev \
  openssh-server \
  openssh-client \
  rsync \
  libzip-dev \
  curl \
  telnet


RUN docker-php-ext-configure \
  gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

# ตามด้วยคำสั่งสำหรับการลง php extension ครับ
RUN docker-php-ext-install \
  bcmath \
  gd \
  intl \
  mbstring \
  mysqli \
  soap \
  xsl \
  json \
  iconv \
  zip
RUN apt-get install -y libpq-dev libsqlite3-dev
RUN pecl install -o -f redis \
  &&  rm -rf /tmp/pear \
  &&  docker-php-ext-enable redis  
RUN docker-php-ext-install pdo_mysql 
# RUN docker-php-ext-install memcached   
RUN docker-php-ext-install intl 
RUN docker-php-ext-install pdo_sqlite \
&& docker-php-ext-enable pdo_sqlite
RUN pecl install xdebug
RUN echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini 
RUN docker-php-ext-enable xdebug
RUN mkdir -p /data/cpm/www
#copy php.ini to container
ADD ./php.ini /usr/local/etc/php/php.ini


# อันนี้ลงเพิ่มเติมคือ composer นั้นเอง เป็นตัว package manager สำหรับการจัดการพวก dependency ของภาษา php ครับ
RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer


