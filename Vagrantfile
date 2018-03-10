$ip01="71"
$ip02="72"
$ip03="73"
#in MSWin it gives you names: VBoxManage.exe list bridgedifs
#$bridge = "Intel(R) Ethernet Connection (2) I219-V"
$bridge = "enp0s31f6"
#$bridge="wlp3s0"
$box = "42n4/pfsense"
#$box="42n4/ubuntu"
$net="192.168.0."
servers=[
  {
    :hostname => "server1",
    :ip => $net+$ip01,
    :bridge => $bridge,
    :box => $box,
    :ram => 2048,
    :cpu => 2,
    :mac => "02002751a1bc"
  }
#  ,
#  {
#    :hostname => "server2",
#    :ip => $net+$ip02,
#    :bridge => $bridge,
#    :box => $box,
#    :ram => 2048,
#    :cpu => 2,
#    :mac => "0200272864d1"
#  },
#  {
#    :hostname => "server3",
#    :ip => $net+$ip03,
#    :bridge => $bridge,
#    :box => $box,
#    :ram => 2048,
#    :cpu => 2,
#    :mac => "020027092383"
#  }
]

Vagrant.configure(2) do |config|
    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]
            node.vm.network "public_network", bridge: machine[:bridge] ,ip: machine[:ip], mac: machine[:mac]
	    #node.vm.network :forwarded_port, guest: 22, host: 2222, guest_ip: "192.168.0.101", host_ip: "localhost", id: "ssh", auto_correct: true
	    #config.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh", disabled: true
            #config.vm.network :forwarded_port, guest: 22, host: 22022, auto_correct: true
            #config.ssh.port = 22
	    #config.ssh.host = "192.168.0.101"
	    config.ssh.password = "vagrant"
	    #node.vm.network "forwarded_port", guest: 8006, host: 8006 if machine[:hostname] == "server1"
            node.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
                vb.customize ["modifyvm", :id, "--nic2", "bridged",  "--nictype2", "82540EM", "--bridgeadapter2", machine[:bridge], "--cableconnected2", "on", "--nicpromisc2", "allow-all" ]
                vb.customize ["modifyvm", :id, "--nic3", "intnet",  "--nictype3", "82540EM", "--intnet3", "intnet0", "--cableconnected3", "on", "--nicpromisc3", "allow-all" ]
                vb.customize ["modifyvm", :id, "--nic4", "intnet",  "--nictype4", "82540EM", "--intnet4", "intnet0", "--cableconnected4", "on", "--nicpromisc4", "allow-all" ]
            end
        end
    end
  config.vm.provision "shell",
    run: "once",
    inline: "ls -l"
end
