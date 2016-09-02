
$init = <<SCRIPT
  sudo aptitude update
  sudo DEBIAN_FRONTEND=noninteractive aptitude install -y build-essential fakeroot debhelper autoconf automake libssl-dev graphviz \
   python-all python-qt4 python-twisted-conch libtool git tmux vim python-pip python-paramiko \
   python-sphinx oracle-java8-installer ant
  sudo pip install alabaster
  sudo pip install requests
  sudo pip install jprops
  sudo pip install pytest
  sudo aptitude install -y openjdk-8-jdk
  echo 'export JAVA_HOME="/usr/lib/jvm/default-java"' >> ~/.profile
  source ~/.profile
SCRIPT

$ovs = <<SCRIPT
  wget http://openvswitch.org/releases/openvswitch-2.3.2.tar.gz
  tar xf openvswitch-2.3.2.tar.gz
  pushd openvswitch-2.3.2
  DEB_BUILD_OPTIONS='parallel=8 nocheck' fakeroot debian/rules binary
  popd
  sudo dpkg -i openvswitch-common*.deb openvswitch-datapath-dkms*.deb python-openvswitch*.deb openvswitch-pki*.deb openvswitch-switch*.deb openvswitch-controller*.deb
  rm -rf *openvswitch*
SCRIPT

$mininet = <<SCRIPT
  git clone git://github.com/mininet/mininet
  pushd mininet
  git checkout -b 2.2.1 2.2.1
  ./util/install.sh -fn
  popd
SCRIPT

$mininext = <<SCRIPT
  # Install MiniNExT
  sudo apt-get install -y help2man python-setuptools

  git clone https://github.com/USC-NSL/miniNExT.git miniNExT/
  cd miniNExT
  git checkout 1.4.0
  sudo make install
SCRIPT

$quagga = <<SCRIPT
  # Install Quagga
  sudo apt-get install -y quagga
SCRIPT

$floodlight = <<SCRIPT
 # Install Floodlight
 git clone git://github.com/floodlight/floodlight
 pushd floodlight
 sudo ant
 popd
SCRIPT

$ryu = <<SCRIPT
  DEBIAN_FRONTEND=noninteractive sudo aptitude install -y python-lxml python-pbr python-greenlet
  sudo pip install six==1.9.0 networkx ryu
  git clone https://github.com/osrg/ryu # Demo apps and stuff
SCRIPT


$odl = <<SCRIPT
  wget https://nexus.opendaylight.org/content/groups/public/org/opendaylight/integration/distribution-karaf/0.3.1-Lithium-SR1/distribution-karaf-0.3.1-Lithium-SR1.tar.gz
  tar xf *Lithium*.tar.gz
  rm *Lithium*.tar.gz
SCRIPT

$onos = <<SCRIPT
  wget http://downloads.onosproject.org/release/onos-1.3.0.tar.gz
  # sudo dpkg -i onos*.deb
  tar xf onos-1.3.0.tar.gz
  rm onos-1.3.0.tar.gz
SCRIPT

$cleanup = <<SCRIPT
  aptitude clean
  rm -rf /tmp/*
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/wily64"

  config.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
      v.customize ["modifyvm", :id, "--memory", "2048"]
  end

  ## Guest config
  config.vm.hostname = "mininet"
  # config.vm.network :private_network, ip: "192.168.0.100"
  config.vm.network :forwarded_port, guest:6653, host:6653 # OpenFlow
  config.vm.network :forwarded_port, guest:8181, host:8181 # Web UI
  config.vm.network :forwarded_port, guest:8080, host:8080 # ODL REST API

  ## Provisioning
  config.vm.provision :shell, privileged: false, :inline => $init
  config.vm.provision :shell, privileged: false, :inline => $ovs
  config.vm.provision :shell, privileged: false, :inline => $mininet
  config.vm.provision :shell, privileged: false, :inline => $mininext
  config.vm.provision :shell, privileged: false, :inline => $quagga
  config.vm.provision :shell, privileged: false, :inline => $floodlight
  #config.vm.provision :shell, privileged: false, :inline => $ryu
  #config.vm.provision :shell, privileged: false, :inline => $odl
  #config.vm.provision :shell, privileged: false, :inline => $onos
  config.vm.provision :shell, :inline => $cleanup

  ## SSH config
  config.ssh.forward_x11 = false

end
