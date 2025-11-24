# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"

  # --- MÁQUINA 1: WEB A ---
  config.vm.define "web1" do |web1|
    web1.vm.network "private_network", ip: "192.168.33.10"

    web1.vm.synced_folder ".", "/vagrant"

    web1.vm.provider "virtualbox" do |vb|
      vb.name = "web1"
      vb.memory = "512"
    end

    web1.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "provision/site.yml"
      ansible.extra_vars = { 
        "mensaje" => "Hola desde el Servidor 1",
        "mi_rol" => "web" 
      }
    end
  end

  # --- MÁQUINA 2: WEB B ---
  config.vm.define "web2" do |web2|
    web2.vm.network "private_network", ip: "192.168.33.11"

    web2.vm.synced_folder ".", "/vagrant"
    
    web2.vm.provider "virtualbox" do |vb|
      vb.name = "web2"
      vb.memory = "512"
    end

    web2.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "provision/site.yml"
      ansible.extra_vars = { 
        "mensaje" => "Hola desde el Servidor 2",
        "mi_rol" => "web" 
      }
    end
  end

  # --- MÁQUINA 3: LOAD BALANCER ---
  config.vm.define "lb" do |lb|
    lb.vm.network "private_network", ip: "192.168.33.100"

    lb.vm.network "forwarded_port", guest: 80, host: 8090
    lb.vm.network "forwarded_port", guest: 1936, host: 1936

    lb.vm.synced_folder ".", "/vagrant"
    
    lb.vm.provider "virtualbox" do |vb|
      vb.name = "lb"
      vb.memory = "512"
    end

    lb.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "provision/site.yml"
      ansible.extra_vars = { 
        "mi_rol" => "lb" 
      }
    end
  end

  # --- MAQUINA 4: DB ---
  config.vm.define "db" do |db|
    db.vm.network "private_network", ip: "192.168.33.20"

    db.vm.synced_folder ".", "/vagrant"
    
    db.vm.provider "virtualbox" do |vb|
      vb.name = "db"
      vb.memory = "512"
    end

    db.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "provision/site.yml"
      ansible.extra_vars = { 
        "mi_rol" => "database",
        "ansible_python_interpreter" => "/usr/bin/python3"
      }
    end
  end

  # --- MAQUINA 5: MONITORING (prometheus + grafana) ---
  config.vm.define "monitor" do |mon| 
    mon.vm.network "private_network", ip: "192.168.33.50"
    mon.vm.host_name = "monitor"

    #grafana
    mon.vm.network "forwarded_port", guest: 3000, host: 3000
    #prometheus
    mon.vm.network "forwarded_port", guest: 9090, host: 9090

    mon.vm.synced_folder ".", "/vagrant"
    
    mon.vm.provider "virtualbox" do |vb|
      vb.name = "monitor"
      vb.memory = "1024"
    end

    mon.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "provision/site.yml"
      ansible.extra_vars = { 
        "mi_rol" => "monitor_server",
        "ansible_python_interpreter" => "/usr/bin/python3"
      }
    end
  end


end
