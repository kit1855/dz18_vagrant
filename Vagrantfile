ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'
ENV['VAGRANT_EXPERIMENTAL'] = 'disks'

Vagrant.configure("2") do |config|
  config.vm.define "vm-vagrant" do |srv|
    srv.vm.box = "ubuntu/jammy64"
    srv.vm.hostname = "srv-1"
    srv.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    srv.vm.disk :disk, size: "1GB", name: "disk1"
    srv.vm.disk :disk, size: "1GB", name: "disk2"
    srv.vm.network "forwarded_port", guest: 80, host: 8080
    srv.vm.provision "shell", inline: <<-SHELL
      mkfs.ext4 /dev/sdc
      mkfs.ext4 /dev/sdd
      mkdir /mnt/disk1
      mkdir /mnt/disk2
      mount /dev/sdc /mnt/disk1
      mount /dev/sdd /mnt/disk2
      echo "UUID=$(blkid -s UUID -o value /dev/sdc) /mnt/disk1 ext4 defaults 0 2" >> /etc/fstab
      echo "UUID=$(blkid -s UUID -o value /dev/sdd) /mnt/disk2 ext4 defaults 0 2" >> /etc/fstab
    SHELL
  end
end
