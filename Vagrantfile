Vagrant.configure("2") do |config|

  ###################################################################################
  config.vm.define "samba-fileserver" do |node|

    # which image to use
    node.vm.box = "opensuse/Leap-15.4.x86_64"

    # sizing of the VMs
    node.vm.provider "libvirt" do |lv|
      lv.random_hostname = false
      lv.memory = 2048
      lv.cpus = 2
    end

    # set the hostname
    node.vm.hostname = "samba-fileserver"

    # disable shared folders
    node.vm.synced_folder ".", "/vagrant", disabled: true
    node.vm.provision "ansible" do |ansible|
      ansible.limit = "all"
      ansible.playbook = "ansible/playbook-all_nodes.yml"
    end # node.vm.provision

  end # config.vm.define

end # Vagrant.configure
