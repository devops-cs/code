Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.hostname = "myprecise.box"
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "public_network"
  
  config.vm.provider "virtualbox" do |vb|
		vb.customize [
      "modifyvm", :id,
      "--cpuexecutioncap", "50",
      "--memory", "256",
    ]
   end
  config.vm.provision "shell", inline: <<-SHELL
	yum update all
  SHELL
end
