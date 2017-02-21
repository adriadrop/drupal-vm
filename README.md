
### This project aims to make spinning up a simple local Drupal test/development environment incredibly quick and easy, and to introduce new developers to the wonderful world of Drupal development on local virtual machines (instead of crufty old MAMP/WAMP-based development).

We won't go into details of Vagrat setup and how it works in this doc, for that use 
(https://www.vagrantup.com/docs/)
(http://docs.drupalvm.com/en/latest/)

Quick setup of vagrant and VBox and Lelo and Foreo sites 

  1. Install [Vagrant](https://www.vagrantup.com/downloads.html) and [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
  2. Download or clone this project to your workstation.
  3. 'cd' into this project directory and run 'vagrant plugin install vagrant-hostsupdater'
  4. 'cd' into this project directory and run 'vagrant plugin install vagrant-auto_network'
  5. 'cd' into this project directory and run 'vagrant up'.
  6. This project directory is synched all the time, so we will put foreo and lelo sites into this directory, create "lelo" folder and
  git clone all the files into it, do the same for "foreo"
  7. Go into the newly created directory and go to sites/default/files/ and run "rsync -a --delete --exclude warranty_claims --exclude private/smart_ip -e ssh git@web04.foreo.com:/var/www/html/lelo/sites/default/files/. ."  to sync all the needed files, do the same for Foreo.
  8. Go to the sited/default and fetch settings.php from dev server and copy it here, set the user/pass drupal/drupal and DB's lelo_vagrant and foreo_vagrant
  9. Download foreo and lelo live DB's from develop.lelo.com/bup/lelo_hex.sql.gz and dev.foreo.com/bup/foreo.sql.gz to project folder
  10. We need to ssh to VM, so we run "vagrant ssh" and enter our project directory, here we will find synched downloads of databases
  and we need to import them so lets run "gunzip < lelo_hex.sql.gz | mysql -u drupal -pdrupal lelo_vagrant" and for foreo
  "gunzip < foreo.sql.gz | mysql -u drupal -pdrupal foreo_vagrant"  after some time (a lot :( ) you will have almost everything
  11. We still need to create CSS so go into sites/all/themes/lelo_theme and foreo_theme and first run "npm install" so you get locall node_modules. If you are still logged in with "vagrant ssh" you could have run that there, we have installed node.js grunt, compass, zen grids. If you have all that on your host machine, you don't need to connect with ssh.
  12. You should be ready to run both sites now locally as lelo.vagrant  and foreo.vagrant
  
  

Some vagrant commands to check out in docs are
-vagrant up
-vagrant halt
-vagrant reload
-vagrant destroy
-vagrant global-status
-vagrant status


Notes:

  - If having problems with setup, take into account server is running varnish, so even if you fix a problem host could show you some error, so run "vagrant ssh" and then "sudo service varnish restart" to clear varnish
  - **For faster provisioning** (macOS/Linux only): *[Install Ansible](http://docs.ansible.com/intro_installation.html) on your host machine, so Drupal VM can run the provisioning steps locally instead of inside the VM.*
  - **For stability**: Because every version of VirtualBox introduces changes to networking, for the best stability, you should install Vagrant's `vbguest` plugin: `vagrant plugin install vagrant-vbguest`.
  - **NFS on Linux**: *If NFS is not already installed on your host, you will need to install it to use the default NFS synced folder configuration. See guides for [Debian/Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-14-04), [Arch](https://wiki.archlinux.org/index.php/NFS#Installation), and [RHEL/CentOS](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-centos-6).*


