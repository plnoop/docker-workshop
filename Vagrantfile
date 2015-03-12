Vagrant.require_version ">= 1.7.2"

# change default synced_folder for convenience
SYNCED_FOLDER = "/home/vagrant/docker-workshop"

# expose ports from guest to host for convenience
FORWARDED_PORT_RANGE = 10080..10100

# external provision script files
PROVISION_SCRIPTS = [ "provision/setup-docker-tools.sh", "provision/setup-env.sh", "provision/setup-hosts.sh" ]


Vagrant.configure(2) do |config|

    config.vm.define "main", primary: true do |node|

        node.vm.box = "williamyeh/ubuntu-trusty64-docker"
        node.vm.box_version = ">= 1.5.0"

        node.vm.network "private_network", ip: "10.0.1.10"

        for i in FORWARDED_PORT_RANGE
            node.vm.network "forwarded_port", guest: i, host: i
        end

        node.vm.synced_folder ".", SYNCED_FOLDER

        for f in PROVISION_SCRIPTS
            node.vm.provision "shell", path: f
        end

        node.vm.provider "virtualbox" do |vb|
            vb.customize ["modifyvm", :id, "--memory", "1024"]
            #vb.customize ["modifyvm", :id, "--memory", "2048"]
        end

    end


    config.vm.define "alice" do |node|

        node.vm.box = "williamyeh/ubuntu-trusty64-docker"
        node.vm.box_version = ">= 1.5.0"
        node.vm.network "private_network", ip: "10.0.1.11"

        node.vm.synced_folder ".", SYNCED_FOLDER

        for f in PROVISION_SCRIPTS
            node.vm.provision "shell", path: f
        end

    end


    config.vm.define "bob" do |node|

        node.vm.box = "williamyeh/ubuntu-trusty64-docker"
        node.vm.box_version = ">= 1.5.0"

        node.vm.network "private_network", ip: "10.0.1.12"

        node.vm.synced_folder ".", SYNCED_FOLDER

        for f in PROVISION_SCRIPTS
            node.vm.provision "shell", path: f
        end

    end


    config.vm.define "centos" do |node|
        node.vm.box = "chef/centos-5.11"
        node.vm.network "private_network", ip: "10.0.1.30"

        node.vm.synced_folder ".", SYNCED_FOLDER

        # [NOTE] unmark this while benchmarking VM startup time
        #node.vm.box_check_update = false

        node.vm.provider "virtualbox" do |vb|
            vb.customize ["modifyvm", :id, "--memory", "256"]
        end
    end



    config.vm.define "registry" do |node|

        node.vm.box = "williamyeh/docker-workshop-registry"
        node.vm.box_version = ">= 3.0.0"

        node.vm.network "private_network", ip: "10.0.1.200"

        node.vm.synced_folder ".", SYNCED_FOLDER

        for f in PROVISION_SCRIPTS
            node.vm.provision "shell", path: f
        end

    end


end
