#!/usr/bin/env ruby

require_relative './data/ansible/utils/key_authorization'

vagrant_config_version = 2
box_type = 'ubuntu/trusty64'
ip_address = '192.168.99.10'
rsa_key = '~/.ssh/vagrant_rsa.pub'
host_name = 'docker.dev'

ports = [
  { 'guest' => 8080, 'host' => 80 },
  { 'guest' => 8091, 'host' => 8091 },  # server
  { 'guest'=> 4984, 'host' => 4984 }, # gateway
  { 'guest' => 4985, 'host' => 4985 } #gateway
]

Vagrant.configure(vagrant_config_version) do | config |

  authorize_key_for_root config, rsa_key

  config.vm.box = box_type
  config.vm.box_check_update = false

  if Vagrant.has_plugin?("vagrant-librarian-chef")
    config.librarian_chef.enabled = false
  end

  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end

  config.vm.network 'private_network', ip: ip_address
  config.vm.hostname = host_name

  # forward given ports
  ports.each do | port |
    config.vm.network 'forwarded_port', guest: port['guest'], host: port['host']
  end

  config.vm.provider 'virtualbox' do | vb |
    vb.memory = 2048
    vb.cpus = 2
  end

end
