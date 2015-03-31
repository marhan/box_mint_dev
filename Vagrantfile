# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "mudfly/salty-mint"

  config.vm.box_check_update = false

  config.vm.provider "virtualbox" do |vb|
     vb.gui = true
  end
  
  #if Vagrant.has_plugin?("vagrant-cachier")
  #  config.cache.scope = :box    
  #end

  #if Vagrant.has_plugin?("vagrant-vbguest")
  #  config.vbguest.auto_update = true
  #  config.vbguest.no_remote = true
  #end

  #if Vagrant.has_plugin?("vagrant-proxyconf")
  #  config.proxy.http     = "http://192.168.0.2:3128/"
  #  config.proxy.https    = "http://192.168.0.2:3128/"
  #  config.proxy.no_proxy = "localhost,127.0.0.1,.example.com"
  #end
  
  ## VM Configurations
  config.vm.define :mintVM do |mintVM|
    mintVM.vm.network :forwarded_port, guest: 22, host: 2201, auto_correct: true
    mintVM.vm.network :forwarded_port, guest: 80, host: 8080, auto_correct: true
    mintVM.vm.hostname = 'mintVM'
    mintVM.vm.synced_folder "salt/roots/", "/srv/"
    
    mintVM.vm.provider "virtualbox" do |v|
      v.name = "mintVM"
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all", "--memory", "2048"]
    end
	
    mintVM.vm.provision :salt do |config|
      config.minion_config = "salt/minion"
      config.run_highstate = true
      config.verbose = true
    end
   
  end
end
