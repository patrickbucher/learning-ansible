Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.network :forwarded_port, id: "ssh", guest: 22, host: 2202, host_ip: "127.0.0.1"
  config.vm.network :forwarded_port, id: "http", guest: 80, host: 8080, host_ip: "127.0.0.1"
  config.vm.network :forwarded_port, id: "https", guest: 443, host: 8443, host_ip: "127.0.0.1"
end
