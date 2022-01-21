# Description
Dockerized PHP web app development environment (based on LAMP stack), that include multiple PHP version:

- `docker-compose.yml` contains PHP v7.2
- `docker-compose-php5.6.yml` contains PHP v5.6
- `docker-compose-hhvm3.27.yml` contains HHVM v3.27

Useful for PHP development that require testing on both PHP v5.6 and v7.2.

I personally use this to do plugin/module development for CMSes like Wordpress, Prestashop, WHMCS, Opencart, Magento, etc. Can be used for framework like CI, Laravel, Slim, etc. This dev env allow me to directly test compatibility for PHP v5 & v7, or satisfy requirement of CMS that require specific PHP version.

# Requirements
- Install Docker
-  On docker Mac: Allow the folder of this project to be mounted as volume, go to: `Preference > Filesharing`. Add this project folder.

# Usage
Choose one of below (depending on which PHP version you want to run. Option A is most recommended):

#### A) Running PHP 7.2
Run docker compose and build `docker-compose up --build -d` (only needed 1st time)
Next time you can just run `docker-compose up -d`

#### B) Running PHP 5.6
Run docker compose and build `docker-compose -f docker-compose-php5.6.yml up --build -d` (only needed 1st time)
Next time you can just run `docker-compose -f docker-compose-php5.6.yml up -d`

#### C) Running HHVM v3.27
Run docker compose and build `docker-compose -f docker-compose-hhvm3.27.yml up --build -d` (only needed 1st time)
Next time you can just run `docker-compose -f docker-compose-hhvm3.27.yml up -d`

Docker compose will run 2 containers: `php_web` container and `db` (mysql) container.

### Accessing
- Start writing (or placing) your php web app in `/web_volume` folder
- Apache PHP (and your web app) are accessible on `http://localhost:11080`
- Mysql / DB:
  - hostname `db` port `3306` from docker container (and php webapp that you place inside web_volume).
    - So for example, if you place WordPress (WP) instalation in web_volume folder, when WP installation/configuration asked for DB connection you can use these (default) values. Mysql db hostname `db`; port `3306`; username `user1`; password `root`; database `db1`.
  - hostname `localhost` port `13306` from outside container.
    - Useful if you want to access the DB from Mysql Desktop app client or client that is from outside the container network.
- Sample PHP files are included, you can access using web browser:
  - http://localhost:11080/info.php - php info
  - http://localhost:11080/adminer.php - database management 
    - Sample credentials to login to DB from Adminer:
      - Server `db` (because the app is trying to connect from internal php_web container)
      - Username `root`
      - Password `root`

### Stopping
- To stop, run `docker-compose stop`
- To stop and remove container, run `docker-compose down`

### Advanced Access
- If you need bash/terminal access to the container (just like ssh), you can run:
  - Php container `docker exec -it php_web bash`
  - Mysql container `docker exec -it db bash` 
- The default user for mysql have limited access, to grant all privilege :
  1. Login mysql as root `docker-compose exec db sh -c 'export MYSQL_PWD="$MYSQL_ROOT_PASSWORD"; mysql'`.
  2. Grant all DB access `GRANT ALL PRIVILEGES ON *.* TO 'user1';`

# Configurations
- Config for php.ini and apache .conf are available on `/config_override`

- Database credential are in `/docker-compose.yml` file, like:
```
...
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: db1
      MYSQL_USER: user1
      MYSQL_PASSWORD: root
...
```

# Files
- Web file for your PHP app can be put on `/web_volume/`
- Mysql database will be saved on `/mysql_volume/`

# Logs
- To see logs and errors, run `docker-compose logs -f`

# Notes
- This is intended for development purpose, may not be suitable for production use.
- Extension installed on PHP container is making the container size huge and slow to build. But those extension is commonly required by CMS. You can manually edit `Dockerfile`s to suit your needs.
- The advantage of using container based PHP environment is that (suppose you have written/installed your PHP web app within web file directory, & mysql db file is saved on db directory, then) you can just copy it to other machine/PC/VM and run the same docker container, and the app & db state will be the same. You can continue to have your app working. Without worrying about system environment/dependency differences (as they are handled by the container).
- Important Changes:
  - php_web container port changed from `10080` to `11080`.
    - Previously php_web container was using port 10080, but Chrome (and other) browsers [started blocking that port](https://chromestatus.com/feature/6510270304223232) (`ERR_UNSAFE_PORT`). So php_web container port is now changed to 11080. Less convenience but unfortunately required to work around it.

# Advanced: Further Developing This Repo/Tool
This section is for maintainer/developer of the repo, not for end user.
- When you change/edit Dockerfile(s), you should build the container before running docker compose.

## TODO
- update default docker-compose.yml PHP version to latest 7.4
- should add PHP version 8.
