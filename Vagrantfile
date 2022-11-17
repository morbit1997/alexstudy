# Массив из хешей, в котором задаются настройки для каждой виртуальной машины
MACHINES = {
  :ubuntu_client => {
    :box_name => "ubuntu-bionic64", # Также, можно указать URL откуда стянуть нужный box если такой есть
    :host_name => "u64client",
    :cpu => 1,
    :ram => 512, # Megabytes
    },
    :ubuntu_server => {
    :box_name => "ubuntu-bionic64", # Также, можно указать URL откуда стянуть нужный box если такой есть
    :host_name => "u64server",
    :cpu => 1,
    :ram => 512, # Megabytes
    }
}
# Входим в Главную конфигурацию vagrant версии 2
Vagrant.configure("2") do |config|
  # Добавить шару между хостовой и гостевой машиной
  #config.vm.synced_folder "F://shared", "/src/shara"
  # Отключить дефолтную шару
  #config.vm.synced_folder ".", "/vagrant", disabled: none

  MACHINES.each do |boxname, boxconfig|                   # Проходим по элементах массива "servers"
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]        # Поднять машину из образа
      box.vm.host_name = boxconfig[:host_name]        # Hostname который будет присвоен VM (самой ОС)
      box.vm.usable_port_range = (2200..2250)         # Пул портов, который будет использоваться для подключения к каждый VM через 127.0.0.1
      # box.vm.network "private_network", ip: boxname[:ip], name: "VirtualBox Host-Only Ethernet Adapter" # Добавление и настройка внутреннего сетевого адаптера (Intranet)
      #box.vm.network "private_network", ip: boxconfig[:ip_addr]
      box.vm.network "public_network"
      # Тонкие настройки для конкретного провайдера (в нашем случае - VBoxManage)
      box.vm.provider :virtualbox do |vb|
        vb.name = boxconfig[:host_name]     # Можно перезаписать название VM в Vbox GUI
        vb.cpus = boxconfig[:cpu]
        vb.memory = boxconfig[:ram]
      end
      if boxconfig[:host_name] == "u64client"
        box.vm.provision "shell", path: "D:/git_bash_home/script_client.sh"
      else
        box.vm.provision "shell", path: "D:/git_bash_home/script_server.sh"
      end
        # Выполнение настроечных скриптов в ОС
    end
  end
end