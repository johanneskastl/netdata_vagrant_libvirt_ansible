# netdata_with_vagrant_libvirt

This Vagrant setup creates a netdata-server VM and two VMs as "nodes".

The "nodes" are being configured to send metrics to the netdata-server, netdata calls that "streams".

Default OS is openSUSE Leap 15.3, but that can be changed in the Vagrantfile. Please beware, this might break the ansible provisioning which was only tested with openSUSE Leap 15.3.

## Vagrant

1. You need vagrant obviously. And ansible. And git...
2. Fetch the box, per default this is `opensuse/Leap-15.3.x86_64`, using `vagrant box add opensuse/Leap-15.3.x86_64`.
3. Make sure the git submodules are fully working by issuing `git submodule init && git submodule update`
4. Run `vagrant up`
5. Open the netdata-server's IP address in your browser using port 19999 and you will find the netdata Dashboard.
6. In the netdata dashboard, check the small `>` sign on the left, where you can switch between the metrics from the netdata-server machine and the other two nodes.
7. Party!

## Disabling the Ansible provisioning

In case you do not want Ansible to install netdata (because you want to install it yourself), just comment out the following lines in the `Vagrantfile`:
```
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.limit = "all"
      ansible.groups = {
        "netdata_servers"  => [ "netdata-server" ],
        "netdata_nodes"   => [ "netdatanode1", "netdatanode2" ]
      }
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision
```
