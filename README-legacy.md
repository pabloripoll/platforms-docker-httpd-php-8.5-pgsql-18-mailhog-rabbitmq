<div style="with:100%;height:auto;text-align:center;">
    <img src="">
</div><!-- HEADER -->

# APACHE 2.4 PHP 8.4 - Postgres 17.5
<br>

## Start

Copy the `./resources/automation/local/Makefile` int root directory to manage platform automated commands
```bash
$ cp ./resources/automation/local/Makefile .
```
<br><br>

## APIREST
<br>

### Creating the APIREST container

- https://wiki.alpinelinux.org/wiki/Apache
- https://wiki.alpinelinux.org/wiki/Apache_with_php-fpm
- https://wiki.alpinelinux.org/wiki/Nginx_with_PHP#Nginx_with_PHP

Apache modules
```bash
/var/www $ sudo httpd -M
```

· Container space: 877.1MB ~ aprox.

· Read .env.example file and copy it into an .env file with the variables updated if it is required.

· Copy the `./platform/nginx-php/docker/config.sample` directory into `./platform/nginx-php/docker/config`

· Open `./platform/nginx-php/docker/config/supervisor/supervisord.conf` and edit the `unix_http_server` block if the CAAS USER and/or GROUP have been changed
```bash
[unix_http_server]
file=/run/supervisord.sock
chmod=0770
chown=myproj:myproj
```

· Open `./platform/nginx-php/docker/config/php/php.ini` and update it if .env APIREST_CAAS_MEM has been changed
```bash
# Memory limit
memory_limit = 512M
```

· Set the root .env variables into the NGINX-PHP platform environment variables
```bash
$ make apirest-set
```

· An .env file must has been created in the platform/docker directory. With that, the container can be created
```bash
$ make apirest-create
```

· There are the minimal base files as welcome page for checking out container is up and running, visiting by the browser as for e.g.: `http:127.0.0.1:7501`
<br>

### Building the APIREST project

As Composer is installed in the container, it ca be installed any available framework that can run in the current stack.

The `./apirest` directory can be GIT ignored or added as a submodule.

Once the `./apirest` is defined how it is going to be treated, access the container TTY to install the desired/required framework.
```bash
$ make apirest-ssh
```

So, to install a framework, just do:
```bash
$ composer composer create-project [php-framework]/[php-framework]:^24 .
```

*Also the installation and maintenance commands can be automated in the platform Makefile.*

Once the project has been installed destroy de project container *(DO NOT DELETE APIREST DIRECTORY)*, and depending on the PHP framework selected, set the supervisor log file `./platform/nginx-php/docker/config/supervisor/supervisord.conf` in the proper directory. For example, Laravel will be set as follows:
```bash
#logfile=/var/www/supervisord.log
logfile=/var/www/storage/logs/supervisord.log
```
<br><br>

## Create Database container

- https://github.com/docker-library/postgres/blob/master/17/alpine3.22/docker-entrypoint.sh


<br><br><br><br><!-- FOOTER -->
<div style="with:100%;height:auto;text-align:center;">
    <img src="./resources/docs/images/pripoll.jpg">
</div>