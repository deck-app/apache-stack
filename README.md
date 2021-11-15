# Docker image for PHP + Apache

Carefully crafted Docker image for PHP + Apache with PHP 8.0, PHP 7.4, PHP 5.6, Apache.

* Fast and simple PHP extensions installation
* Alpine base image with [PHP.earth PHP repositories](https://docs.php.earth/linux/alpine)
* Multiple PHP versions
* [runit](http://smarden.org/runit/) for running multiple services without overhead
* Optional Composer installation
* Optional PHP installation
* Optimized Docker image sizes


## Docker tags

The following list contains all current Docker tags and what is included in each.


| System | Docker Tag | Features | Size |
| ------ | ---------- | -------- | ---- |
| **PHP 5.6.40**@Alpine 3.5 | [`5.6-apache`](https://github.com/deck-app/apache-stack1/blob/master/Dockerfile-5.6) | Apache 1.12.2, FPM |  |
| **PHP 7.4.25**@Alpine 3.10 | [`7.4-apache`](https://github.com/deck-app/apache-stack1/blob/master/Dockerfile-7.4) | Apache 1.14.2, FPM | [![](https://images.microbadger.com/badges/image/phpearth/php:7.4-apache.svg)](https://microbadger.com/images/phpearth/php:7.4-apache "Image size") |
| **PHP 8.0.12**@Alpine 3.12 | [`8.0-apache`](https://github.com/deck-app/apache-stack1/blob/master/Dockerfile-7.4) | Apache 1.14.2, FPM | [![](https://images.microbadger.com/badges/image/phpearth/php:7.4-apache.svg)](https://microbadger.com/images/phpearth/php:7.4-apache "Image size") |

## Quick usage

### Apache

`Dockerfile` for running Apache HTTP server with PHP FPM:

```Dockerfile
FROM pull dockerstacks/apache2_php:v{PHP Version}


### Composer

To install Composer:

```Dockerfile
FROM dockerstacks/apache2_php:v{PHP Version}

RUN apk add --no-cache composer
```

### PHPUnit

To install PHPUnit:

```Dockerfile
FROM dockerstacks/apache2_php:v{PHP Version}

RUN apk add --no-cache phpunit
```

```Dockerfile
FROM dockerstacks/apache2_php:v{PHP Version}

RUN apk add --no-cache php7.3-sodium php7.3-intl php7.3-pdo_mysql
```

or install them with `pecl`:

```bash
apk add --no-cache php7.3-dev gcc g++
pecl install {extension-name}
```

### PHP ini settings
create application specific `php.ini` and copy the file into docker image:


```ini
# php.ini
memory_limit = 512M
```

```Dockerfile
FROM dockerstacks/apache2_php:v{PHP Version}

COPY php.ini $PHP_INI_DIR/conf.d/my-app.ini
```

### Docker Stack

Docker Stack is way of orchestration of Docker services and simplifies running multiple services of your application. In this example we'll run an Apache web server with PHP 7.3 FPM with `docker-compose.yml` file. In a new project directory create a `Dockerfile`:

```Dockerfile
FROM dockerstacks/apache2_php:v{PHP Version}
```

The `docker-compose.yml` file:

```yml
version: '3'

services:
  app:
    image: dockerstacks/apache2_php:v{PHP Version}
    volumes:
      - .:/var/www/
    ports:
      - mode: host
        target: 80
        published: 80
```

The `index.php` file:

```php
<?php

phpinfo();
```

And there should be `phpinfo()` output visible on `http://localhost`. Make sure there isn't any other service listening on port 80 before running above command.

### PHP 7.0, 7.1, PHP 7.2

To use older versions PHP 7.0, 7.1, or 7.2 use Docker images with `7.0`, `7.1`, or `7.2`:

```Dockerfile
FROM dockerstacks/apache2_php:v{PHP Version}

RUN apk add --no-cache composer
```


