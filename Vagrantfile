# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

current_dir     = File.dirname(File.expand_path(__FILE__))
configs         = YAML.load_file("#{current_dir}/config.yaml")
vagrant_config  = configs['configs'][configs['configs']['use']]

Vagrant.configure(2) do |config|
  config.vm.define "test" do |test|
    test.vm.box = vagrant_config['dist']
    test.vm.hostname = vagrant_config['hostname']
    test.vm.network :private_network, ip: vagrant_config['public_ip']
    test.vm.network :forwarded_port, guest: vagrant_config['guest'], host: vagrant_config['host']
    # Requires 'vagrant plugin install vagrant-triggers'
    config.trigger.before :suspend do
      info "Deleting tmp content before suspending"
      run_remote "sudo rm -rf /tmp/*"
    end
    config.vm.provider "virtualbox" do |v|
      v.memory = vagrant_config['memory']
      v.cpus = vagrant_config['cpu']
    end
  end
end
