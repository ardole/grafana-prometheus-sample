# -*- mode: ruby -*-
# vi: set ft=ruby :

servers = JSON.parse(%{
[
    {
        "name": "ansible-control-machine",
        "box": "centos/7",
        "ram": 1024,
        "cpus": 1,
        "ip": "192.168.48.140",
        "inline-shell": "yum update -y && yum install -y epel-release && yum install -y ansible",
        "synced-folder": {
            "host": "C:\\Workspace",
            "guest": "/workspace"
        }
    },
    {
        "name": "ansible-node-1",
        "box": "centos/7",
        "ram": 1024,
        "cpus": 1,
        "ip": "192.168.48.141",
        "inline-shell": "yum update -y",
        "ports": [
            {
                "host": 9090,
                "guest": 9090
            },{
                "host": 3000,
                "guest": 3000
            }
        ]
    },
    {
        "name": "ansible-node-2",
        "box": "centos/7",
        "ram": 1024,
        "cpus": 1,
        "ip": "192.168.48.142",
        "inline-shell": "yum update -y"
    }
]
})

Vagrant.configure("2") do |config|
  servers.each do |server|
    config.vm.define server['name'] do |srv|
      srv.vm.hostname = server['name']
      srv.vm.box = server['box']
      srv.vm.network "private_network", ip: server['ip']
      if server.has_key?('inline-shell')
        srv.vm.provision "shell", inline: server['inline-shell']
      end
      if server.has_key?('synced-folder')
        srv.vm.synced_folder server['synced-folder']["host"], server['synced-folder']["guest"], type: "virtualbox"
      end
      if server.has_key?('ports')
        server['ports'].each do |port|
            srv.vm.network "forwarded_port", guest: port['guest'], host: port['host']
        end
      end
      srv.vm.provider "virtualbox" do |v|
        v.memory = server['ram']
        v.cpus = server['cpus']
       end
    end
  end
end

