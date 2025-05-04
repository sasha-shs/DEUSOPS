Vagrant.configure("2") do |config|
  config.vm.box = "net9/ubuntu-24.04-arm64"
  config.vm.box_version = "1.1"

#Количество требуемых машин
  SERVERS = 2
#Имя сетевого интерфейса для организации моста
  BRIDGE = "en0: Wi-Fi"

  def create_host(config, hostname, ip)
    config.vm.define hostname do |host|
      host.vm.network "private_network", ip: ip
      host.vm.network "public_network", bridge: BRIDGE
      host.vm.hostname = hostname
      host.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
      end
      host.vm.provision "shell", inline: "apt-get update && apt-get install -y python3-minimal"
      yield host if block_given?
    end
  end

  (1..SERVERS).each do |machine_id|
    create_host(config, "srv#{machine_id}", "192.168.56.#{200+machine_id}")
  end
end
