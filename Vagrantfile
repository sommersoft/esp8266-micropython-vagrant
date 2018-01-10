# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "./vagrant", "/vagrant", create: true
  
  # Optionally share the /home/vagrant/source directory with NFS (only for Mac
  # and Linux host machines, for Windows try SMB below).
  #config.vm.synced_folder "./source", "/home/vagrant/source", type: "nfs", create: true
  
  # Optionally share the /home/vagrant/source directory with SMB (only for Windows
  # host machines, for Mac OSX or Linux try NFS above). ESP open SDK uses symbolic links
  # which cannot be created with an SMB shared folder. Use the first shared folder command
  # below run for the initial 'vagrant up', or anytime you need to use 'git submodule update'
  # in the ESP open SDK folder.
  #config.vm.synced_folder "./source", "/home/vagrant/source", create: true
  config.vm.synced_folder "./source", "/home/vagrant/source", type: "smb", create: true

  # Virtualbox VM configuration.
  config.vm.provider "virtualbox" do |v|
    # Bump the memory allocated to the VM up to 1 gigabyte as the compilation of
    # the esp-open-sdk tools requires more memory to complete.
    v.memory = 1024
    
    # Allow symbolic links for Windows
    v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/vagrant", "1"]
    
    # Enable USB.
    v.customize ["modifyvm", :id, "--usb", "on"]
    
    # Add a USB passthrough for the SAMD21 bootloader VID & PID.
    v.customize ["usbfilter", "add", "0", "--target", :id, "--name", "Adafruit M0 Bootloader", "--vendorid", "0x239a", "--productid", "0x000b"]
  end

  # Provision script to install dependencies used by the esp-open-sdk and
  # micropython tools.  First install dependencies as root.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    echo "Beginning VM provisioning..."
    echo "Installing esp-open-sdk, Espressif ESP-IDF, and micropython dependencies..."
    sudo apt-get update
    sudo apt-get install -y -qq build-essential git make unrar-free unzip bzip2 \
                            autoconf automake libtool gcc g++ gperf \
                            flex bison texinfo gawk ncurses-dev libexpat-dev \
                            python sed libreadline-dev libffi-dev pkg-config \
                            help2man python-dev python-serial wget linux-image-extra-$(uname -r)
    echo "Installing Espressif ESP32 toolchain..."
    cd ~
    wget https://dl.espressif.com/dl/xtensa-esp32-elf-linux64-1.22.0-61-gab8375a-5.2.0.tar.gz 2> /dev/null
    tar xvfz xtensa-esp32-elf-linux64-1.22.0-61-gab8375a-5.2.0.tar.gz
    
    # Link xtensa path to .profile, if it isn't already linked
    if ! grep -s 'PATH=/home/vagrant/xtensa-esp32-elf/bin:\$PATH' "$PROFILE"; then
        echo "Editing $PROFILE to add xtensa path on Terminal launch"
        echo "PATH=/home/vagrant/xtensa-esp32-elf/bin:\$PATH" >> ~/.profile
    fi
    #echo "PATH=/home/vagrant/xtensa-esp32-elf/bin:\$PATH" >> ~/.profile
    
    echo "Installing esp-open-sdk, Espressif ESP-IDF, and micropython source..."
    git clone --recursive https://github.com/pfalcon/esp-open-sdk.git
    git clone --recursive https://github.com/espressif/esp-idf.git
    
    #echo "Finished provisioning, now run 'vagrant ssh' to enter the virtual machine."
    echo "Finished ESP8266 provisioning!"

    echo "Installing ATMEL-SAMD toolchain..."
    sudo add-apt-repository -y ppa:team-gcc-arm-embedded/ppa

    sudo apt-get update -qq
    sudo apt-get install -y -qq python3 gcc-multilib libffi-dev:i386 qemu-system gcc-mingw-w64

    sudo apt-get install -y -qq wx2.8-headers libwxgtk2.8-0 libwxgtk2.8-dev

    sudo apt-get install -y --force-yes gcc-arm-embedded

    echo "Building and installing BOSSA..."
    cd ~
    git clone https://github.com/shumatech/BOSSA.git
    cd BOSSA
    make bin/bossac
    sudo cp bin/bossac /usr/local/bin/

    echo "Cloning CircuitPython source..."
    mkdir -p ~/source
    cd ~/source
    #git clone https://github.com/micropython/micropython.git
    git clone https://github.com/adafruit/circuitpython.git

    cd circuitpython
    git submodule update --init --recursive

    echo "Finished provisioning!  Use the 'vagrant ssh' command to enter VM.  CircuitPython source is in the /home/vagrant/source/circuitpython folder."
  SHELL
  
end
