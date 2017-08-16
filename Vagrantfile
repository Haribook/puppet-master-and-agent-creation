
Vagrant.configure("2") do |config|
	config.vm.define "puppetmaster" do |subconfig|
		subconfig.vm.box = "bento/centos-7.3"
		subconfig.vm.network "private_network", ip: "10.10.10.10"
		subconfig.vm.hostname = "puppetmaster.cisco.com"
		subconfig.vm.provision "shell", inline: <<-SHELL
			yum -y install http://yum.puppetlabs.com/puppetlabs-release-el-7.noarch.rpm
			yum -y install puppet-server
			yum -y install vim
			sed -i '/main/a certname = puppet' /etc/puppet/puppet.conf
			echo "10.10.10.11  puppetagent.cisco.com" >> /etc/hosts
			systemctl start puppetmaster
			systemctl enable puppetmaster
		SHELL
	end
	config.vm.define "puppeagent" do |subconfig|
		subconfig.vm.box = "bento/centos-7.3"
		subconfig.vm.network "private_network", ip: "10.10.10.11"
		subconfig.vm.hostname = "puppetagent.cisco.com"
		subconfig.vm.provision "shell", inline: <<-SHELL
			yum -y install http://yum.puppetlabs.com/puppetlabs-release-el-7.noarch.rpm
			yum -y install puppet
			yum -y install vim
			sed -i '/agent/a server = puppetmaster.cisco.com' /etc/puppet/puppet.conf
			echo "10.10.10.10  puppetmaster.cisco.com" >> /etc/hosts
			systemctl start puppet
			systemctl enable puppet
		SHELL
	end
end
