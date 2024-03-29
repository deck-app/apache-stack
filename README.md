# Apache stack

> Docker-compose project with Alpine linux, Apache and PHP. Super easy to get started and running in a few mins

## Install

### Using DECK

Install apache-stack from the DECK marketplace and follow the instructions on the GUI

### From terminal with Docker

```
git clone https://github.com/deck-app/apache-stack/
cd apache-stack
```

Edit `.env` file to change any settings before installing like php, apache versions etc

```
docker-compose up -d
```
### Modifying project settings
From the DECK app, go to stack list and click on project's `More > configure > Advanced configuration`
Follow the instructions below and restart your stack from the GUI

#### Edit Apache configuration

httpd.conf is located at `./apache/httpd.conf`

#### Editing php.in

PHP ini file is located at `./apache/php_ini/php{YOUR.PHP.VERSION}.ini`

#### Installing / removing PHP extensions

Add / remove PHP extensions from `./apache/Dockerfile-{YOUR.PHP.VERSION}`

```
RUN apk add --update --no-cache bash \
				curl \
				curl-dev \
				php7-intl \
				php7-openssl \
				php7-dba \
				php7-sqlite3 \
```

#### Rebuilding from terminal

You have to rebuild the docker image after you make any changes to the project configuration, use the snippet below to rebuild and restart the stack

```
docker-compose stop && docker-compose up --build -d
```
