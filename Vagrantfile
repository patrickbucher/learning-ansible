Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.network :forwarded_port, host: 8000, guest: 80
  config.vm.network :forwarded_port, host: 8443, guest: 443
  # config.vm.network :private_network, ip: "192.168.56.100"
end
