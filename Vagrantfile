# -*- mode: ruby -*-
# vi: set ft=ruby :
Dotenv.load

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'
VAGRANTFILE_HOST_NAME = "#{ENV['HOST_NAME']}" || 'drone.dev'
GITHUB_REPO =  "#{ENV['GITHUB_REPO']}" || 'kazu69/drone-playground'

$script = <<SCRIPT
wget http://downloads.drone.io/latest/drone.deb
dpkg -i drone.deb
echo 'cd /opt/go/src/$2' >> /home/vagrant/.bashrc
start drone
open http://$1/install
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = 'Official Ubuntu 12.04 daily Cloud Image amd64'
  config.vm.box_url = 'http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box'
  config.vm.network :private_network, ip: '192.168.10.101'
  config.vm.hostname = VAGRANTFILE_HOST_NAME
  config.vm.synced_folder ".", "/opt/go/src/github.com/#{GITHUB_REPO}", nfs: true
  config.ssh.forward_agent = true
  config.vm.network :forwarded_port, guest: 80, host: 80
  config.vm.provision "shell", inline: $script, args: "#{VAGRANTFILE_HOST_NAME} #{GITHUB_REPO}"
end
