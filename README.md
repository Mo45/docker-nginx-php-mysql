# Nginx PHP MySQL

Docker running Nginx, PHP-FPM, MySQL.

## Install prerequisites

To run the docker commands without using **sudo** you must add the **docker** group to **your-user**:

```
sudo usermod -aG docker your-user
```

For now, this project has been mainly created for Unix `(Linux/MacOS)`. Perhaps it could work on Windows.

All requisites should be available for your distribution. The most important are :

* [Git](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/engine/installation/)
* [Docker Compose](https://docs.docker.com/compose/install/)
* [Certbot](https://hub.docker.com/r/certbot/certbot)

Check if `docker compose` is already installed by entering the following command : 

```sh
docker compose
```

On Ubuntu and Debian these are available in the meta-package build-essential. On other distributions, you may need to install the GNU C++ compiler separately.

```sh
sudo apt install build-essential
```

### Images to use

* [Nginx](https://hub.docker.com/_/nginx/)
* [MySQL](https://hub.docker.com/_/mysql/)
* [PHP-FPM](https://hub.docker.com/_/php)

You should be careful when installing third party web servers such as MySQL or Nginx.

This project use the following ports :

| Server     | Port |
|------------|------|
| MySQL      | 8989 |
| Nginx      | 80   |
| Nginx SSL  | 443  |

___

### Project tree

```sh
.
├── data
│   └── db
│       ├── dumps
│       └── mysql
├── docker-compose.yml
├── etc
│   ├── nginx
│   │   ├── default.conf
│   │   └── default.template.conf
│   ├── php
│   │   └── php.ini
│   └── ssl
└── web
    └── public
        └── index.php
```

___

## Configure Nginx With SSL Certificates

1. [HowTo](https://dev.to/mrshanas/https-on-docker-containers-using-nginx-and-letsencrypt-3nfa)

2. Configure Nginx

    Do not modify the `etc/nginx/default.conf` file, it is overwritten by  `etc/nginx/default.template.conf`

    Edit nginx file `etc/nginx/default.template.conf` and uncomment the SSL server section :

    ```sh
    # server {
    #     server_name ${NGINX_HOST};
    #
    #     listen 443 ssl;
    #     fastcgi_param HTTPS on;
    #     ...
    # }
    ```
___

#### Connecting MySQL from [PDO](http://php.net/manual/en/book.pdo.php)

```php
<?php
    try {
        $dsn = 'mysql:host=mysql;dbname=test;charset=utf8;port=3306';
        $pdo = new PDO($dsn, 'dev', 'dev');
    } catch (PDOException $e) {
        echo $e->getMessage();
    }
?>
```
