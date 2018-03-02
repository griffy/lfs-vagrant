# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-16.04"

  config.vm.provider "virtualbox" do |vb|
    build_disk = 'build_disk.vdi'
    unless File.exist?(build_disk)
        vb.customize ['createhd', '--filename', build_disk, '--size', 20000]
    end
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', build_disk]

    vb.gui = false
    vb.cpus = 4
    vb.memory = "4096"
  end

  config.vm.provision "shell", inline: <<-SHELL
    rm /bin/sh && ln -s /bin/bash /bin/sh
    apt-get update
    apt-get install -y build-essential texinfo bison subversion libncurses-dev libxml2 libxml2-utils xsltproc
    mkfs -v -t ext4 /dev/sdb
    echo "export LFS=/mnt/build_dir" >> /root/.bash_profile
    export LFS=/mnt/build_dir
    mkdir -pv $LFS
    mount -v -t ext4 /dev/sdb $LFS
    chown -R vagrant:vagrant $LFS
  SHELL
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    echo "export LFS=/mnt/build_dir" >> /home/vagrant/.bash_profile
    svn co svn://svn.linuxfromscratch.org/ALFS/jhalfs/tags/2.4 jhalfs-2.4
  SHELL
end
