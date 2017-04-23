#!/usr/bin/env ruby

require 'getoptlong'

VAGRANTFILE_API_VERSION = "2"

playbook = 'test.yml'
ansible_groups = %w(container-servers production)

opts = GetoptLong.new(
  # Vagrant native options
  ['--force', '-f', GetoptLong::NO_ARGUMENT],

  # Custom options
  ['--playbook', GetoptLong::OPTIONAL_ARGUMENT],
)
opts.each do |opt, arg|
  case opt
  when '--playbook'
    playbook = arg
  end
end

Vagrant.configure("2") do |config|

    config.vm.define "ansibletest" do |config|
        config.vm.box = 'http://cloud.terry.im/vagrant/archlinux-x86_64.box'
        config.vm.network :forwarded_port, guest: 22, host: 50457
        config.vm.network :forwarded_port, guest: 5000, host: 5000

        config.vm.provision :ansible do |ansible|
          ansible.playbook = playbook
          ansible.inventory_path = 'hosts'
          ansible.extra_vars = {
            override_hosts: 'ansibletest'
          }
        end
    end
end
