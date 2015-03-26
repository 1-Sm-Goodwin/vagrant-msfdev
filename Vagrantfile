# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Creating lots of boxes, running this first time will take around
# Running vagrant provision will be faster in most circumstances

  boxes = [
  { :name => :msfdev, :cpus => 2,:mem => 2048 },
  ]


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    boxes.each do |opts|
        config.vm.define opts[:name] do |config|

            # My VMware box, wont upload it
            config.vm.box       = "kramse/ubuntu-14.04-64"
            # insert some other box, we should make Kali Vagrant boxes
            # perhaps tomorrow

#            config.vm.synced_folder "../../customers", "/customers"
            config.vm.hostname = "%s.vagrant" % opts[:name].to_s
            config.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--cpus", opts[:cpus] ] if opts[:cpus]
                vb.customize ["modifyvm", :id, "--memory", opts[:mem] ] if opts[:mem]
            end
            config.vm.provider "vmware_fusion" do |vb|
                vb.gui = true
                vb.vmx["numvcpus"] = opts[:cpus]
                vb.vmx["memsize"] = opts[:mem]
            end
            config.vm.provision "ansible" do |ansible|
                ansible.playbook = "playbook-%s.yml" % opts[:name].to_s
                # if you need vars inside the playbooks
                ansible.extra_vars = {
                  root_password_ubuntu: "xxx",
                  location_name: "local",
                  country_code: "dk",
                  city: "Development",
                }
            end
       end
    end
end
