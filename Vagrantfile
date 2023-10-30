# -*- mode: ruby -*-
# vi: set ft=ruby :

nodes = [
  { :hostname => "vag-md", :mac => "525400836f54", :ram => 4096 },
  { :hostname => "vag-ps1", :mac => "525400836f51", :ram => 2048 },
  { :hostname => "vag-ps2", :mac => "525400836f52", :ram => 2048 },
  { :hostname => "vag-ps3", :mac => "525400836f53", :ram => 2048 }

]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |node_config|
      node_config.vm.box = "generic/rocky9"
      node_config.vm.network :public_network, :mac => node[:mac], bridge: "enp0s20f0u2"
      node_config.vm.provision "file", source: "/home/vagrant/.ssh/id_ed25519.pub", destination: "/home/vagrant/.ssh/authorized_keys"

      node_config.vm.hostname = node[:hostname]
      node_config.vm.provider :virtualbox do |domain|
        domain.memory = node[:ram]
        domain.cpus = 1
      end
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "Testpoint-MaDDashbuild.yml"
    ansible.groups = {
      "testpoint" => ["vag-ps1", "vag-ps2", "vag-ps3"],
      "md-arch"   => ["vag-md"]
    }
  end
end
