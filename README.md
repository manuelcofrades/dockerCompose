#Practica de docker compose
## Con automatización del proceso para ubuntu22.04:
* Se crea un directorio en local
* En ese directorio local se crea el docker-compose.yml con el siguiente contenido, que será el que levante los contenedores docker
dentro de la máquina virtual

Archivo docker-compose.yml
```
version: '3.1'
services:
  wordpress:
    container_name: servidor_wp
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: asdasd
      WORDPRESS_DB_NAME: bd_wp
    ports:
      - 80:80
    volumes:
      - wordpress_data:/var/www/html/wp-content
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: bd_wp
      MYSQL_USER: user_wp
      MYSQL_PASSWORD: asdasd
      MYSQL_ROOT_PASSWORD: asdasd
    volumes:
      - mariadb_data:/var/lib/mysql
volumes:
    wordpress_data:
    mariadb_data:
```

* Se crea el vagrantfile con el siguiente contenido:
```
# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
    config.vm.box = "bento/ubuntu-22.04"
    # puertos host y máquina virtualizada
    config.vm.network :forwarded_port, host: 8080, guest: 80
    # config.vm.synced_folder './', '/vagrant', SharedFoldersEnableSymlinksCreate: false
    # require plugin https://github.com/leighmcculloch/vagrant-docker-compose
    config.vagrant.plugins = "vagrant-docker-compose"
    
    # copiamos la carpeta de los estáticos dentro de la máquina. 
    # Para usar rsync, tiene que estar instalado en el host
    # https://learn.microsoft.com/en-us/windows/wsl/install

    # config.vm.provision "file", source: "./web", destination: "/home/vagrant/web"
    # config.vm.synced_folder "./web", "/home/vagrant/web", type: "rsync"  #, rsync__exclude: ".git/"
    # install docker and docker-compose
    config.vm.provision :docker
    config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", rebuild: true, run: "always"
    # config.vm.provision :shell, path: "provision.sh"  
  end
```
* Se levanta el entorno , desde consola en el directorio con 
vagrant up

* Tengo en http://localhost:8080/ corriendo la aplicación


## Como no me funciona en mi equipo el Sistema Operativo Ubuntu22.04 con Vagrant, levanto con el Ubuntu20.04

* Se crea un directorio en local
* En ese directorio local se crea el docker-compose.yml con el siguiente contenido, que será el que levante los contenedores docker
dentro de la máquina virtual

Archivo docker-compose.yml
```
version: '3.1'
services:
  wordpress:
    container_name: servidor_wp
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: asdasd
      WORDPRESS_DB_NAME: bd_wp
    ports:
      - 80:80
    volumes:
      - wordpress_data:/var/www/html/wp-content
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: bd_wp
      MYSQL_USER: user_wp
      MYSQL_PASSWORD: asdasd
      MYSQL_ROOT_PASSWORD: asdasd
    volumes:
      - mariadb_data:/var/lib/mysql
volumes:
    wordpress_data:
    mariadb_data:
```

* Se crea el vagrantfile con el siguiente contenido:
```
# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  # puertos host y máquina virtualizada
  config.vm.network :forwarded_port, host: 8080, guest: 80
  
  # config.vm.synced_folder './', '/vagrant', SharedFoldersEnableSymlinksCreate: false
  # require plugin https://github.com/leighmcculloch/vagrant-docker-compose
  #config.vagrant.plugins = "vagrant-docker-compose"
  
  # copiamos la carpeta de los estáticos dentro de la máquina. 
  # Para usar rsync, tiene que estar instalado en el host
  # https://learn.microsoft.com/en-us/windows/wsl/install

  # install docker and docker-compose
  config.vm.provision "docker"
end
```
*Si incluyo en este sistema operativo que me instale el docker-compose da fallo y no me levanta la máquina virtual

* Se levanta el entorno , desde consola en el directorio con 
vagrant up

*Tengo que instalar el docker-compose, para ello

vagrant ssh

Instalo el docker-compose:
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
*Creo el archivo docker-compose.yml, 
nano docker-compose.yml y pego el contenido:
```
version: '3.1'
services:
  wordpress:
    container_name: servidor_wp
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: asdasd
      WORDPRESS_DB_NAME: bd_wp
    ports:
      - 80:80
    volumes:
      - wordpress_data:/var/www/html/wp-content
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: bd_wp
      MYSQL_USER: user_wp
      MYSQL_PASSWORD: asdasd
      MYSQL_ROOT_PASSWORD: asdasd
    volumes:
      - mariadb_data:/var/lib/mysql
volumes:
    wordpress_data:
    mariadb_data:
```
* Ahora levanto los contenedores
docker-compose up -d

* Tengo en http://localhost:8080/ corriendo la aplicación

![Imagen1](https://github.com/manuelcofrades/dockerCompose/blob/main/wordpress.png)

