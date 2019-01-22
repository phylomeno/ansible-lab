# Ansible Test Lab
This lab can be used to experiment around with the power of [Ansible](https://www.ansible.com/).
[Vagrant](https://www.vagrantup.com/) is used to setup the following set of VMs for the lab environment:

| Node               | Are             |
| ------------------ | --------------- |
| ansible-master     | ubuntu/xenial64 |
| ubuntu-node1       | ubuntu/xenial64 |
| ubuntu-node2       | ubuntu/xenial64 |
| ubuntu-node3       | ubuntu/xenial64 |
| centos-node1       | centos/7        |
| centos-node2       | centos/7        |
| centos-node3       | centos/7        |
| debian-node1       | debian/jessie64 |
| debian-node2       | debian/jessie64 |
| debian-node3       | debian/jessie64 |


## Lab Setup
### Setup Vagrant
On a Ubuntu based machine the following commands can be used to install the latest version of Vagrant

    wget -c https://releases.hashicorp.com/vagrant/2.0.3/vagrant_2.0.3_x86_64.deb
    sudo dpkg -i vagrant_2.0.3_x86_64.deb`

After successful installation also add the vagrant-hosts plugin which will be used to register all Vagrant machines in hosts file.

    vagrant plugin install vagrant-hosts


### Start the Lab environment
In the folder with the Vagrant file the following command will setup all the VMs. This may take a while.

    vagrant up
    
Now one can ssh into the ansible-master VM.
           
    vagrant ssh ansible-master
    
Last step before playing around is installing latest Ansible version.

    sudo apt-add-repository ppa:ansible/ansible
    sudo apt update
    sudo apt install ansible sshpass
 
### Verify setup
While still logged into ansible-master perform the following commands to ensure the setup is running.

    cd /opt/ansible
    ansible -i production all -m ping -k
    
The command will ask for the ssh password which is `vagrant`. After entering this Ansible will perform an Ansible-ping with all nodes.

## Experiment 1: Distribute SSH Keys
In this experiment we want to add an SSH key to the authorized_keys of each server. Let us first generate some ssh keys.

    ssh-keygen
    ansible-playbook -i production distribute-ssh-key.yml -k
   
After this step you should be able to perform your Ansible commands without providing an SSH command.

    ansible -i production all -m ping
   
Interesting to see as well that Ansible modules behave idempotent. Performing the same step again should have no effect.

    ansible-playbook -i production distribute-ssh-key.yml
    
# Experiment 2: Install open-vm-tools

    ansible-playbook -i production install-vmware-tools.yml
    
# Experiment 3: Facts gathering

To see what facts are available for a certain host use the following command:

    ansible -i production -m setup ubuntu-node1
    
To collect certain facts for all hosts use the following

    ansible -i production -a 'filter=ansible_memtotal_mb' -m setup all
    ansible -i production -a 'filter=ansible_kernel' -m setup all

