# samba_fileserver_vagrant_libvirt_ansible

This Vagrant setup creates two VMs to play around with Samba as a file server.
The VM called `samba-server` is being configured as the server, the other one
(`samba-client1`) as a client.

Default OS is openSUSE 15.4. Although that can be changed in the Vagrantfile,
please beware that this will break the Ansible provisioning.

Ansible will install all the tools and setup the Samba server with a very (!)
basic configuration. It will also generate three users in the OS and in Samba:

- testuser1
- testuser2
- testuser3

(Passwords will be the same as the usernames, so do not use this anywhere near
production!)

## Vagrant

1. You need vagrant obviously. And ansible. And git...
1. Fetch the box, per default this is `opensuse/Leap-15.4.x86_64`, using
   `vagrant box add opensuse/Leap-15.4.x86_64`.
1. Make sure the git submodules are fully working by issuing `git submodule init
   && git submodule update`
1. Run `vagrant up`
1. Connect to the client VM using `vagrant ssh sambaclient1`. Try to mount the
   share called `vagrant-libvirt`:

   ```bash
   sambaclient1:~ # smbclient -L samba-server.training.b1-systems.de -U testuser1
   Password for [WORKGROUP\testuser1]:

           Sharename       Type      Comment
           ---------       ----      -------
           vagrant-libvirt Disk      Sample share
           IPC$            IPC       IPC Service (Samba 4.15.13-git.636.53d93c5b9d6150400.3.23.1-SUSE-oS15.0-x86_64)
   SMB1 disabled -- no workgroup available
   sambaclient1:~ # mount -t cifs -o username=testuser1 //samba-server.training.b1-systems.de/vagrant-libvirt /mnt/
   🔐 Password for testuser1@//samba-server.training.b1-systems.de/vagrant-libvirt:  *********
   sambaclient1:~ # ll /mnt/
   total 0
   drwxr-xr-x 2 root root 0 May 14 14:41 testuser1
   drwxr-xr-x 2 root root 0 May 14 14:41 testuser2
   drwxr-xr-x 2 root root 0 May 14 14:41 testuser3
   sambaclient1:~ #
   ```

1. Party!

## Disabling the Ansible provisioning

In case you do not want Ansible to install teleport (because you want to install
it yourself), just comment out the following lines in the `Vagrantfile`:

```hcl
    node.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/playbook-all_nodes.yml"
    end # node.vm.provision
```

You also find all of the playbooks in the `ansible` folder.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via kastl@b1-systems.de.
