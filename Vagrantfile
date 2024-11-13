NUM_MASTER_NODE = 1
NUM_WORKER_NODE = 2
IP_NW = "192.168.56.13"
MASTER_IP_START = 1
NODE_IP_START = 2

Vagrant.configure("2") do |config|

config.vm.box = "ubuntu/bionic64"

config.vm.box_check_update = false

(1..NUM_MASTER_NODE).each do |i|
    config.vm.define "kubermaster" do |node|
        node.vm.provider "virtualbox" do |vb|
            vb.name = "kubermaster"
            vb.memory = 2048
            vb.cpus = 2
        end
        node.vm.hostname = "kubermaster"

        node.vm.network :private_network, ip: IP_NW + "#{MASTER_IP_START + i}"

        node.vm.provision "shell", path: "./setup-config.sh"
        node.vm.provision "shell", inline: <<-SHELL
        echo "123" | passwd --stdin root
        sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
        systemctl reload sshd
        SHELL
    end
end

(1..NUM_WORKER_NODE).each do |i|
    config.vm.define "kubenode0#{i}" do |node|
        node.vm.provider "virtualbox" do |vb|
            vb.name = "kubenode0#{i}"
            vb.memory = 1024
            vb.cpus = 1
        end
        node.vm.hostname = "kubenode0#{i}"
        node.vm.network :private_network, ip: IP_NW + "#{NODE_IP_START + i}"
        node.vm.provision "shell", path: "./setup-config.sh"
        node.vm.provision "shell", inline: <<-SHELL
        echo "123" | passwd --stdin root
        sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
        systemctl reload sshd
        SHELL
    end
end
end