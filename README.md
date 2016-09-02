# vagrant-mininet

This Vagrantfile tries to be an all-inclusive script to provide an
OpenFlow 1.3 and Floodlight testing environment in a VM. Difficult
to install tools such as Mininet and OVS are packaged in.

Tools included:

- Mininet 2.1.0
- MiniNExT 1.4.0
- OpenVSwitch 2.3.0
- Quagga
- Floodlight

## Usage

Start with the installation of VirtualBox and Vagrant. Next, download the
Vagrantfile and run `vagrant up` from the folder it is stored in.

After the script finishes its job, `vagrant ssh` from the corresponding folder
should let you in the VM.

Windows users may want to install `msysgit` distribution for a
seamless ssh experience.
