# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
VAGRANT_API_VERSION = '2'

config_file = File.expand_path(File.join(File.dirname(__FILE__), 'vagrant_vars.yml'))
settings = YAML.load_file(config_file)

HOSTNAME_PREFIX	= settings['hostname_prefix'] ? settings['hostname_prefix'] + '-' : ""
BOX             = settings['vagrant_box']
MEMORY          = settings['memory']

Vagrant.configure(VAGRANT_API_VERSION) do |config|

  config.vm.box = BOX

  (1..2).each do |i|

    config.vm.define "#{HOSTNAME_PREFIX}node#{i}" do |node|

      node.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--name", "#{HOSTNAME_PREFIX}node#{i}"]
        vb.customize ["modifyvm", :id, "--memory", "#{MEMORY}"]
      end

      node.vm.provision "file", source: "config-node#{i}.yaml", destination: "/tmp/config.yaml"
      node.vm.provision "shell" do |s|
        s.inline = "cp /tmp/config.yaml /var/lib/rancher/k3os/config.yaml && reboot"
        s.upload_path = "/home/rancher/vagrant-shell"
        s.privileged = true
      end

    end

  end
end
