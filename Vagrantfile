# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANT_API_VERSION         = "2"
ENV['VAGRANT_NO_PARALLEL']  = 'yes'

VAGRANT_BOX         = "generic/ubuntu2204"
VAGRANT_BOX_VERSION = "4.2.10"
CPUS_MASTER_NODE    = 2
CPUS_WORKER_NODE    = 2
MEMORY_MASTER_NODE  = 8196
MEMORY_WORKER_NODE  = 2048
WORKER_NODES_COUNT  = 3
NETWORK_PREFIX      = "172.16.16"


Vagrant.configure(VAGRANT_API_VERSION) do |config|

  config.vm.box               = VAGRANT_BOX
  config.vm.box_check_update  = false
  config.vm.box_version       = VAGRANT_BOX_VERSION

  config.vm.provision "shell", path: "bootstrap.sh"

  # Kubernetes Master Server
  config.vm.define "kmaster" do |node|

    node.vm.hostname = "kmaster.example.com"
    node.vm.network "private_network", ip: "#{NETWORK_PREFIX}.100"

    node.vm.provider :virtualbox do |v|
      v.name    = "kmaster"
      v.memory  = MEMORY_MASTER_NODE
      v.cpus    = CPUS_MASTER_NODE
    end

    node.vm.provider :libvirt do |v|
      v.memory  = MEMORY_MASTER_NODE
      v.nested  = true
      v.cpus    = CPUS_MASTER_NODE
    end

    node.vm.provision "shell", path: "bootstrap_kmaster.sh"

  end


  # Kubernetes Worker Nodes
  (1..WORKER_NODES_COUNT).each do |i|

    config.vm.define "kworker#{i}" do |node|

      node.vm.hostname = "kworker#{i}.example.com"
      node.vm.network "private_network", ip: "#{NETWORK_PREFIX}.10#{i}"

      node.vm.provider :virtualbox do |v|
        v.name    = "kworker#{i}"
        v.memory  = MEMORY_WORKER_NODE
        v.cpus    = CPUS_WORKER_NODE
      end

      node.vm.provider :libvirt do |v|
        v.memory  = MEMORY_WORKER_NODE
        v.nested  = true
        v.cpus    = CPUS_WORKER_NODE
      end

      node.vm.provision "shell", path: "bootstrap_kworker.sh"

    end

  end

end
