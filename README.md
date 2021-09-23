# magento-2-docker
Magento 2 docker setup

## Resources
* [Shawn Abrahamson's Tutorial on Magento 2 with Docker](https://www.magemodule.com/all-things-magento/magento-2-tutorials/docker-magento-2-development/)
* [Raroslav Rogoza's Tutorial on Magento 2 with Docker in WSL2](https://www.atwix.com/magento/magento-2-with-docker-for-windows-and-wsl-2/)

## Docker dev setup
* Create a path for your docker configurations in your filesystem
  * Example: `/home/ubuntu/dev/docker`

## Moving to a docker setup
* If you move your filesystem, make backup of `app/etc` configuration files
* Export current databases
* Create a `docker-compose.yml` in your docker configurations directory (or create a subdirectory first)
  * [See sample file](https://github.com/Luc4G3r/magento-2-docker/blob/main/docker-compose/docker-compose-sample.yaml)
  * This file needs some editing depending on your environment wishes of course
* The volume path of apache container should point to your current magento installation
* Close all services which run on the ports you defined in your `docker-compose.yml`
  * You might want to disable autostart of those services with `sudo systemctl disable {service}`
* Build the container with `docker-compose up -d --build`
* Verify by opening your host url
* Access your container with `docker exec -it {container_name} bash`
* The sample configuration will place the magento project in linked directory called `app`
  * From there you can run `bin/magento` commands
* To get the magento instance running, update configurations in `app/etc/env.php`

### WSL2
* Make sure the url is added to Windows host file (if `/etc/host` file is generated from Windows)
  * You don't have to change anything, if you want to keep the current url
* Consider installing Docker Desktop on Windows
  * It supports running and managing containers in multiple WSL2 instances
