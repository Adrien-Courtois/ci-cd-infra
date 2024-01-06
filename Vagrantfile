# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.env.enable
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y figlet
    mkdir -p "/root/.ssh"
    echo #{ENV['SSH_PUBKEY']} > /root/.ssh/authorized_keys
  SHELL

  config.vm.define "gitlab-pipeline" do |gitlab|
    gitlab.vm.box = "debian/bullseye64"
    gitlab.vm.hostname = "gitlab-pipeline"
    gitlab.vm.box_url = "debian/bullseye64"
    gitlab.vm.network :private_network, ip: "192.168.56.2"
    gitlab.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--name", "gitlab-pipeline"]
      v.customize ["modifyvm", :id, "--cpus", "4"]
    end
    gitlab.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
      figlet "gitlab-pipeline" > /etc/motd
      service ssh restart
    SHELL
  end

  config.vm.define "webserver" do |webserver|
    webserver.vm.box = "debian/bullseye64"
    webserver.vm.hostname = "webserver"
    webserver.vm.box_url = "debian/bullseye64"
    webserver.vm.network :private_network, ip: "192.168.56.3"
    webserver.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id, "--name", "webserver"]
      v.customize ["modifyvm", :id, "--cpus", "1"]
    end
    webserver.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
      figlet "webserver" > /etc/motd
      service ssh restart
    SHELL
  end

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
  end
end
