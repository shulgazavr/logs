servers = [
    { :hostname => 'elk', :ips => '192.168.31.201', :ram => 5120, :cpuz => 2 },
    { :hostname => 'web', :ips => '192.168.31.202', :ram => 762, :cpuz => 1 },
]

Vagrant.configure(2) do |config|
    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = 'centos/7'
            node.vm.box_version = "2004.01"
            node.vm.hostname = machine[:hostname]
            node.vm.network :public_network, ip: machine[:ips], bridge: "wlp58s0"
            node.vm.provider "virtualbox" do |vb|
                vb.memory = machine[:ram]
                vb.cpus = machine[:cpuz]
            end
#            node.vm.provision "file", source: "id_rsa.pub", destination: "/home/vagrant/id_rsa.pub"
            node.vm.provision "shell", inline: <<-SHELL
                mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
                sed -i '65s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
                systemctl restart sshd
#                sudo cat id_rsa.pub >> /root/.ssh/authorized_keys
#                rm id_rsa.pub
            SHELL
            node.vm.provision :ansible do |ansible|
                ansible.playbook = "playbooks/playbook-#{node.vm.hostname}.yml"
            end
        end
        
    end
end
