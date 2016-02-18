# -*- mode: ruby -*-
# vi: set ft=ruby :

# Note - if behind a proxy you need to set http_proxy and https_proxy
# for the pull of the base image to work. Also, this assumes the
# proxy plugin has been installed via
# vagrant plugin install vagrant-proxyconf
#
# You may also need to configure a portforwarding rule at the virtual
# box level to forward port <proxy port> to <proxy port> (guess os host os)
# to get to the proxy server
#

VAGRANTFILE_API_VERSION = "2"

$script = <<SCRIPT
wget -qO- https://get.docker.com/ | sh
sudo usermod -aG docker vagrant
curl -L https://github.com/docker/compose/releases/download/1.6.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
#Install of dvol doesn't work during vagrant provisioning - docker is unable to pull from
#docker hub as proxy is not set yet. Just run it the first time you log in
echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~"
echo "Now install dvol via 'sudo curl -sSL https://get.dvol.io |sh'"
SCRIPT

machine_ip = "172.20.21.22"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end

  if ENV['http_proxy'].nil? == false
      puts 'Setting http proxy'
      config.proxy.http = ENV['http_proxy']
      config.proxy.https = ENV['http_proxy']
      config.proxy.no_proxy="localhost," + machine_ip + ",/var/run/docker.sock"
    else
      puts 'Environment does not specify http_proxy value...'
  end


  config.vm.network "private_network", ip: machine_ip
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update=false
  config.vm.provision "shell", inline: $script
end
