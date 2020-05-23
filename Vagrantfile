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
    config.vm.define "cnssc" do |cnssc|
       cnssc.vm.box = "fedora/32-cloud-base"
       # Should you require machines to share a private network
       # Note, you will need to create the network first within
       # your provider (VirtualBox / Libvirt etc)
       # cnssc.vm.network :private_network, ip: "10.0.0.#{i}1"
       # cnssc.vm.network "forwarded_port", guest: 443, host: "844#{i}"
       cnssc.vm.hostname = "tekton-dev"
       if defined? (repo)
         cnssc.vm.synced_folder "#{repo}", "/home/vagrant/cnssc-dev", type: "sshfs"
       end
       cnssc.vm.provider "virtualbox" do |v|
        v.memory = "#{memory}"
        v.cpus = "#{cpus}"
       end
       cnssc.vm.provider "libvirt" do |vb|
         vb.random :model => 'random'
         vb.memory = "#{memory}"
         vb.cpus = "#{cpus}"
       end
       cnssc.vm.provision "ansible_local" do |ansible|
           ansible.playbook = "playbook.yml"
           ansible.galaxy_role_file = "requirements.yml"
       end
    end
end
