ansible-webstack-php-fpm-nginx-mysql-centos6
============================================

THIS IS A COMBINATION OF VAGRANT + ANSIBLE

You can also use the ansible playbooks only.

Full Web Stack for deploying PHP Applications - Repo Support, Mysql, PHP-FPM5.5, Nginx, Redis, Varnish, Gearman, RabbitMQ

============================================

I. Requirements

	+ Vagrant (with Virtual Box)
	+ Ansible

II. Install Vagrant for Local Development

	
	1. Download Virtual Box to machine

		https://www.virtualbox.org/wiki/Downloads

	2. Download Vagrant

		www.vagrantup.com/downloads.html 

	3. Create a folder to store your virtual machines of vagrant

		Windows: C:\vms
		Mac: /Users/Name/vms

		(These are optional folders)

	4. Download file Centos and store into the above folder

		https://github.com/2creatives/vagrant-centos/releases/download/v6.5.1/centos65-x86_64-20131205.box 

	5. Create a 'dev' folder inside above folder

	6. Go to dev folder, and run

		vagrant init
		vagrant box add dev /Users/Name/vms/centos65.box



	7. Open file vms/dev/vagrantfile and edit

		- Change config.vm.box from "base" to "dev"
		- uncomment this line: config.vm.network :private_network, ip: "192.168.33.10" 
		- uncomment this line:  config.vm.network :forwarded_port, guest: 80, host: 8080 

		- If you have many VM Instances, remember to change above IP and host:8080

	8. Boot VM, cd to dev folder.

		vagrant up

	9. SSH into your machine

		vagrant ssh

	10. Switch to root

		sudo -s

	11. Config Sync Folder

		config.vm.synced_folder "/Users/Name/Source", "/srv/www", :mount_options => ['dmode=777', 'fmode=777']

		("/Users/Name/Source" is your machine folder)
		("/srv/www" is vm folder)


	12. Other commands

		- Turn on VM: vagrant up
		- Shutdown VM: vagrant halt
		- Reload VM: vagrant reload
		- Run with provision (like ansible): vargrant provision


III. Ansible Configuration


	1. Open Vagrant File

	config.vm.provision "ansible" do |ansible|
    	ansible.playbook = "ansible/install.yml" #Configure to use which playbook
    	ansible.inventory_path = "ansible/hosts" #Configure to use which hosts
    	ansible.verbose = true
    	ansible.sudo = true
  	end


  	2. Note: If you encounter this error

  	ERROR: problem running path_to/ansible/hosts --list ([Errno 8] Exec format error)

  	do this: 

  	chmod -x path_to/ansible/hosts

  	3. If you have run swap module, remember to comment # it if you need to do other provison module later



