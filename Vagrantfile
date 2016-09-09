# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

settings = YAML.load_file("Settings.yaml")

ansiblePlaybook = "ansible/playbook.yml"

# To be able to set hostname on Ubuntu 16.04, we need Vagrant ver 1.8.3
# or greater. Also see https://github.com/mitchellh/vagrant/issues/7288
Vagrant.require_version ">= 1.8.3"

Vagrant.configure("2") do |config|

  # Set auto_update to true, to ensure
  # we get the latest Guest Additions
  config.vbguest.auto_update = true

  # Set the hostname
  hostname = settings["hostname"] ||= "vagrantbox"

  # Prevent TTY Errors
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

  # Get the box
  config.vm.box = "ubuntu/xenial64"
  # config.vm.box_url = "http://cloud-images.ubuntu.com/xenial/current/xenial-server-cloudimg-amd64-vagrant.box"

  # Configure the hostname
  config.vm.hostname = hostname

  # Configure private network IP
  config.vm.network :private_network, ip: settings["ip"] ||= "172.16.16.16"

  # Configure VirtualBox settings
  config.vm.provider "virtualbox" do |vb|
    vb.name = hostname
    vb.customize ["modifyvm", :id, "--ostype", "Ubuntu_64"]
    vb.customize ["modifyvm", :id, "--memory", settings["memory"] ||= "1024"]
    vb.customize ["modifyvm", :id, "--cpus", settings["cpus"] ||= "2"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  # Configure port forwarding
  if settings.has_key?("ports")
    settings["ports"].each do |port|
      if port.has_key?("id")
        config.vm.network "forwarded_port", guest: port["guest"], host: port["host"], protocol: port["protocol"] ||= "tcp", id: port["id"], auto_correct: true
      else
        config.vm.network "forwarded_port", guest: port["guest"], host: port["host"], protocol: port["protocol"] ||= "tcp", auto_correct: true
      end
    end
  end

  # Configure agent forwarding to use our existing key pair
  config.ssh.forward_agent = true

  # Configure public key for SSH access
  if settings.include? 'authorize'
    config.vm.provision "shell" do |s|
      s.inline = "echo $1 | grep -xq \"$1\" /home/ubuntu/.ssh/authorized_keys || echo \"\n$1\" | tee -a /home/ubuntu/.ssh/authorized_keys"
      s.args = [File.read(File.expand_path(settings["authorize"]))]
    end
  end

  # Copy SSH private keys to the box
  if settings.include? 'keys'
    settings["keys"].each do |key|
      config.vm.provision "shell" do |s|
        s.privileged = false
        s.inline = "echo \"$1\" > /home/ubuntu/.ssh/$2 && chmod 600 /home/ubuntu/.ssh/$2"
        s.args = [File.read(File.expand_path(key)), key.split('/').last]
      end
    end
  end

  # Disable the default Vagrant folder mapping.
  # This doesn't apply to ubuntu/xenial64, which
  # uses 'ubuntu' as the default account
  #config.vm.synced_folder ".", "/vagrant", disabled: true

  # Configure synced folder mappings
  if settings.include? 'folders'
    settings["folders"].each do |folder|
      mount_opts = []

      if (folder["type"] == "nfs")
          mount_opts = folder["mount_opts"] ? folder["mount_opts"] : ['actimeo=1']
      end

      config.vm.synced_folder folder["map"], folder["to"], type: folder["type"] ||= nil, mount_options: mount_opts
    end
  end

  # App settings pre-processing
  if settings.include? 'apps'
    settings["apps"].each do |app|

      # Format app names
      if app.has_key?("name")
        app["app_name"] = (app["name"].gsub(" ", "-") || app["name"]).downcase
      end

      # Set doc root
      if app.has_key?("app_path") && !app.has_key?("doc_root")
        if (app["type"] == "laravel" || app["type"] == "rails")
          app.store("doc_root", app["app_path"] + "/public")
        else
          app.store("doc_root", app["app_path"])
        end
      end

    end
  end

  # Use Ansible automation
  if File.exists? ansiblePlaybook then
    config.vm.provision :ansible do |ansible|
      config.ssh.username = "ubuntu"
      ansible.playbook = ansiblePlaybook
      ansible.extra_vars = {
        hostname: hostname,
        apps: settings["apps"] ||= nil,
        mysql_root_password: settings["mysql_admin_passwords"]["root"] ||= "vagrant",
        mysql_dba_password: settings["mysql_admin_passwords"]["dba"] ||= "vagrant"
      }
    end
  end

end