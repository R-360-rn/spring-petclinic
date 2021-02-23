# -*- mode: ruby -*-
# vi: set ft=ruby :
instances = {app: {hostname: "APP-VM", RAM: "1024", CPU: "2", address: "192.168.50.4", ports: {guest: 8080, host: 8080}, playbook: "app.yml"}, 
             db: {hostname: "DB-VM", RAM: "2048", CPU: "4", address: "192.168.50.5", playbook: "db.yml"}}
Vagrant.configure("2") do |config|
  instances.each do |k,v|
    config.vm.define v.dig(:hostname) do |node|
      node.vm.box = "ubuntu/trusty64"
      if v.dig(:hostname) == "APP-VM"
        node.vm.network "forwarded_port", guest: v.dig(:ports,:guest), host: v.dig(:ports,:host), host_ip: "127.0.0.1"
      end
      node.vm.network "private_network", ip: v.dig(:address)
      node.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "playbooks/" + v.dig(:playbook)
      end
      node.vm.hostname = v.dig(:hostname)
      node.vm.provider "virtualbox" do |vb|
        vb.memory = v.dig(:RAM)
        vb.cpus = v.dig(:CPU)
        vb.name = v.dig(:hostname)
      end
    end
  end
end
