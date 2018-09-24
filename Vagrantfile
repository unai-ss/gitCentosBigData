Vagrant.configure("2") do |config|
# Muestra GUI del proceso de creacion del VirtualBox
#config.vm.provider :virtualbox do |vb|
#  vb.gui = true
#end


  config.vm.define "Ansible" do |app|
    app.vm.box = "CentosBox/Centos-7-v7.4-Minimal-CLI"
    app.vm.box_version = "17.11.24"
    app.vm.define "Ansible"
    app.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "192.168.1.206"
    app.vm.network "private_network", ip: "172.16.1.4"
    app.vm.synced_folder "./",  "/tmp/sync"
    app.vm.box_url = "\n"
    app.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 256]
      v.customize ["modifyvm", :id, "--name", "Ansible"]
      v.customize ["modifyvm", :id, "--usb", "on"]
      v.customize ["modifyvm", :id, "--usbehci", "off"]
      v.customize ["modifyvm", :id, "--macaddress1", "auto"]
    end
    app.vm.provision "shell", inline: <<-SHELL
	sudo hostnamectl set-hostname ansible.external
	sudo yum install ansible vim git chrony dnsmasq ntpdate squid nginx -y
	sudo git clone https://github.com/dcos-labs/ansible-dcos.git
        sudo eval "$(ssh-agent)"
        sudo ntpdate -s hora.roa.es
	sudo cp /tmp/sync/server_files/chrony.conf /etc/
        sudo systemctl enable chronyd && sudo systemctl start chronyd
	sudo cp /tmp/sync/server_files/dnsmasq.con /etc/dnsmasq/
        sudo systemctl enable dnsmasq && sudo systemctl start dnsmasq
	sudo sysctl net.ipv4.ip_forward = 1
	sudo cp /tmp/sync/server_files/squid.conf /etc/squid/
	sudo systemctl enable squid && sudo systemctl start squid
	sudo cp /tmp/sync/server_files/default.conf /etc/nginx/conf.d/
	sudo systemctl enable nginx && sudo systemctl start nginx
	sudo cp /tmp/sync/server_files/ansible.cfg /etc/
        sudo adduser centos
        sudo usermod -aG wheel centos
        sudo cp -R /tmp/sync/server_files/ansible /etc/
     	sudo cp -R /tmp/sync/ansible_docs /home/vagrant/
	sudo chmod -R vagrant:vagrant /home/vagrant/ansible_docs
    SHELL
  end


  config.vm.define "Bootstrap" do |app|
    app.vm.box = "CentosBox/Centos-7-v7.4-Minimal-CLI"
    app.vm.box_version = "17.11.24" 
    app.vm.define "Bootstrap"
    app.vm.network "private_network", ip: "172.16.1.1"
    app.vm.synced_folder "./",  "/tmp/sync"
    app.vm.box_url = "\n"
    app.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "Bootstrap"]
      v.customize ["modifyvm", :id, "--usb", "on"]
      v.customize ["modifyvm", :id, "--usbehci", "off"]
      v.customize ["modifyvm", :id, "--macaddress1", "auto"]
    end
    app.vm.provision "shell", inline: <<-SHELL
	sudo hostnamectl set-hostname bootstrap.external
#	sudo cp /tmp/sync/nodes_files/proxy.sh /etc/profile.d/
	sudo yum install python-pip
	sudo pip install --upgrade pip
    SHELL
  end

  config.vm.define "Master" do |db|
    db.vm.box = "CentosBox/Centos-7-v7.4-Minimal-CLI"
    db.vm.box_version = "17.11.24"
    db.vm.define "Master"
    db.vm.network "private_network", ip: "172.16.1.2"
    db.vm.synced_folder "./",  "/tmp/sync"
    db.vm.box_url = "\n"

    db.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "Master"]
      v.customize ["modifyvm", :id, "--usb", "on"]
      v.customize ["modifyvm", :id, "--usbehci", "off"]
      v.customize ["modifyvm", :id, "--macaddress1", "auto"]
    end
    db.vm.provision "shell", inline: <<-SHELL
	sudo hostnamectl set-hostname masterlb.external
#        sudo cp /tmp/sync/nodes_files/proxy.sh /etc/profile.d/
    SHELL
 end


  config.vm.define "Agent" do |control|
    control.vm.box = "CentosBox/Centos-7-v7.4-Minimal-CLI"
    control.vm.box_version = "17.11.24"
    control.vm.define "Agent"
    control.vm.network "private_network", ip: "172.16.1.3"
    control.vm.synced_folder "./",  "/tmp/sync"
    control.vm.box_url = "\n"

    control.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--usbehci", "off"]
      v.customize ["modifyvm", :id, "--macaddress1", "auto"]
    end
    control.vm.provision "shell", inline: <<-SHELL
	sudo hostnamectl set-hostname agent.external
        sudo cp /tmp/sync/nodes_files/proxy.sh /etc/profile.d/
    SHELL

  end

end
