Install vagrant
wget -c https://releases.hashicorp.com/vagrant/2.0.3/vagrant_2.0.3_x86_64.deb
sudo dpkg -i vagrant_2.0.3_x86_64.deb

Host plugin
vagrant plugin install vagrant-hosts

vagrant up
           
vagrant ssh ansible-master
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible
sudo apt install sshpass
