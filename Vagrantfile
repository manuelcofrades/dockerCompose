
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