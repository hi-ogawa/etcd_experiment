# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "hiogawa/ubuntu_docker"

  config.vm.define "vm0" do |m|
    m.vm.network 'private_network', ip: '192.168.33.11'
  end

  config.vm.define "vm1" do |m|
    m.vm.network 'private_network', ip: '192.168.33.12'
  end

  config.vm.define "vm2" do |m|
    m.vm.network 'private_network', ip: '192.168.33.13'
  end

  config.ssh.pty = true
  config.vm.provision 'shell' do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
    s.inline = <<-SHELL
      echo #{ssh_pub_key} >> /home/ubuntu/.ssh/authorized_keys
    SHELL
  end
end
