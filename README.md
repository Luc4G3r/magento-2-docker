# magento-2-docker
Magento 2 docker setup instructions and configurations

## List of contents
* [Images contained in this repository](#images-contained-in-this-repository)
* [Resources](#resources)
* [Docker dev setup](#docker-dev-setup)
* [Moving to a docker setup](#moving-to-a-docker-setup)
* [Multiple projects](#multiple-projects)

### Images contained in this repository
* magento2-project
  * `docker-compose.yml` configuration for an apache2 (magento 2) dev environment
* mariadb
  * `docker-compose.yml` configuration for a local mariadb mysql server
* mailhog
  * `docker-compose.yml` configuration for a local smtp server
* elasticsearch
  * `docker-compose.yml` configuration for a local elasticsearch server
* redis
  * `docker-compose.yml` configuration for a local redis cache server

### Resources
* [Shawn Abrahamson's Tutorial on Magento 2 with Docker](https://www.magemodule.com/all-things-magento/magento-2-tutorials/docker-magento-2-development/)
* [Raroslav Rogoza's Tutorial on Magento 2 with Docker in WSL2](https://www.atwix.com/magento/magento-2-with-docker-for-windows-and-wsl-2/)

### Docker dev setup
* Clone this repository
* Copy `*/.env.sample` files to `*/.env`
* Copy `magento2-project/data/magento/env.php.sample` to `magento2-project/data/magento/env.php`
* Copy `magento2-project/data/php/additional.php.ini.sample` to `magento2-project/data/php/additional.php.ini`
* Configure copied files for needed images
* Remove unwanted dependencies from `magento2-project/docker-compose.yml`
* Run `docker-compose up -d --build` in directories of needed images
  * Depending images (magento2) will need to be build after dependencies

### Moving to a docker setup
* Make backup of `app/etc` configuration files in magento project
  * Copy the `app/etc/env.php` to `magento2-project/data/magento/env.php`
* Close all services which run on the ports you defined in your `docker-compose.yml`
  * You might want to disable autostart of those services with `sudo systemctl disable {service}`
* Follow [docker dev setup](#docker-dev-setup) steps
* Import current databases
  * Export current databases with `mysqldump`
  * Build mariadb image with `docker-compose`
  * Enter mariadb container with `docker exec -it mariadb bash`
  * Import your exported databases into mariadb docker container
    * `docker exec -i {CONTAINER_NAME} mysql -u {USER} -p < db.sql`, enter password
* Build other needed containers with `docker-compose up -d --build`
* Verify by opening your host url
* Access your containers with `docker exec -it {container_name} bash`
* The sample configuration will place the magento project in linked directory called `app`
  * From there you can run `bin/magento` commands
* To get the magento instance running, update configurations in `app/etc/env.php`
  * Other containers adapter configurations will have to be applied here

#### WSL2
* Make sure all urls are added to Windows host file (if `/etc/host` file is generated from Windows)
* Consider installing Docker Desktop on Windows
  * Supports running and managing containers in multiple WSL2 instances
  * Supports PHPStorm Service tab configuration, PHP interpreter, xdebug and much more  
    (Docker Desktop will handle Windows to WSL2 path translations, very handy!)

### Multiple projects
* `docker stop {apache2 container}`
* Copy `magento2-project` to another directory
* Append files and `.env` configuration
* Run `docker-compose up -d --build` in new directory