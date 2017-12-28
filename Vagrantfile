# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 tw=0 et :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# cluster configure
boxes = {
  "ubuntu-1604" => { :box => "bento/ubuntu-16.04",      :ip => "10.0.77.12", :cpu => "1", :ram => "1024" },
  "debian-9"    => { :box => "bento/debian-9",          :ip => "10.0.77.14", :cpu => "1", :ram => "1024" },
  "centos-7"    => { :box => "bento/centos-7",          :ip => "10.0.77.16", :cpu => "1", :ram => "1024" },
  "suse-12"     => { :box => "bento/opensuse-leap-42",  :ip => "10.0.77.18", :cpu => "1", :ram => "1024" },
}

Vagrant.configure("2") do |config|
  ###############################################################################
  # Global plugin settings                                                      #
  ###############################################################################
  #if Vagrant.has_plugin?("vagrant-cachier")
  #  config.cache.scope = :box
  #  config.cache.auto_detect = true
  #  config.cache.synced_folder_opts = {
  #    type: :nfs,
  #    mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
  #  }
  #end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.no_remote   = true
    config.vbguest.auto_update = false
  end
  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled           = false
    config.hostmanager.manage_host       = true
    config.hostmanager.ignore_private_ip = false
  end

  # box
  config.vm.box_check_update  = false
  # ssh
  config.ssh.insert_key       = false
  config.ssh.forward_agent    = true
  # synced folders
  config.vm.synced_folder ".", "/vagrant", disabled: true

  boxes.each_with_index do |(hostname, info), index|
    config.vm.define hostname do |cfg|
      cfg.vm.box     = info[:box]
      cfg.vm.box_url = info[:url]

      # virtualbox
      cfg.vm.provider :virtualbox do |vb|
        vb.name   = "#{hostname}"
        vb.cpus   = info[:cpu]
        vb.memory = info[:ram]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end

      # network
      cfg.vm.network :private_network, ip: "#{info[:ip]}", nictype: "virtio"

      # provision
      if index == boxes.size - 1
        cfg.vm.provision :ansible do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.playbook           = "tests/vagrant.yml"
          ansible.limit              = "all"
          ansible.verbose            = "vv"
          ansible.host_vars          = {
            "ubuntu-1604" => {
              "ansible_python_interpreter" => "/usr/bin/python3"
            },
            "debian-9" => {
              "ansible_python_interpreter" => "/usr/bin/python3"
            }
          }
          ansible.raw_arguments      = [
            "--diff",
          ]
        end
      end
    end
  end
end
