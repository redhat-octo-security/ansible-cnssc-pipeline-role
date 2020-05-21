# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'getoptlong'

opts = GetoptLong.new(
  [ '--instances', GetoptLong::OPTIONAL_ARGUMENT ],
  [ '--repo', GetoptLong::OPTIONAL_ARGUMENT ],
  [ '--cpus', GetoptLong::OPTIONAL_ARGUMENT ],
  [ '--memory', GetoptLong::OPTIONAL_ARGUMENT ]
)

# defaults
cpus = 2
memory = 2048
repo = ''

opts.ordering=(GetoptLong::REQUIRE_ORDER)

opts.each do |opt, arg|
  case opt
    when '--repo'
      repo = arg
    when '--cpus'
      cpus = arg.to_i
    when '--memory'
      memory = arg.to_i
  end
end

Vagrant.configure("2") do |config|
    config.vm.define "tektondev" do |tektondev|
       tektondev.vm.box = "fedora/32-cloud-base"
       # Should you require machines to share a private network
       # Note, you will need to create the network first within
       # your provider (VirtualBox / Libvirt etc)
       # tektondev.vm.network :private_network, ip: "10.0.0.#{i}1"
       # tektondev.vm.network "forwarded_port", guest: 443, host: "844#{i}"
       tektondev.vm.hostname = "tekton-dev"
       if defined? (repo)
         tektondev.vm.synced_folder "#{repo}", "/home/vagrant/tekon-dev", type: "sshfs"
       end
       tektondev.vm.provider "virtualbox" do |v|
        v.memory = "#{memory}"
        v.cpus = "#{cpus}"
       end
       tektondev.vm.provider "libvirt" do |vb|
         vb.random :model => 'random'
         vb.memory = "#{memory}"
         vb.cpus = "#{cpus}"
       end
       tektondev.vm.provision "ansible_local" do |ansible|
           ansible.playbook = "playbook.yml"
           ansible.galaxy_role_file = "requirements.yml"
       end
    end
end
