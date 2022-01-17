## QEMU/KVM

#### basic setup

* install prerequisites:
  * `sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat`
  * `sudo pacman -S ebtables` https://linux.die.net/man/8/ebtables
    * `iptables` for ethernet frames
  * `sudo pacman -S libguestfs`
* edit /etc/libvirt/libvirtd.conf and uncomment:
  * `unix_sock_group = "libvirt"`
  * `unix_sock_ro_perms = "0777"`
* add your user to the correct group
  * `sudo usermod -a -G libvirt $(whoami)`
  * `newgrp libvirt`
    * change group association without full logout/login
* edit `/etc/default/grub`
  * add to `GRUB_CMDLINE_LINUX_DEFAULT` (space separated)
    * `intel_iommu=on` allows virtualization (there is an `amd_` equivalent)
    * `iommu=pt` allows PCI passthrough in some cases
    * for these params to be loaded
      * `sudo grub-mkconfig -o /boot/grub/grub.cfg`
      * `sudo grub-mkconfig -o /boot/efi/EFI/manjaro/grub.cfg`
  * check `sudo dmesg | grep IOMMU` to see if you're doinitrite

#### Articles

* https://octetz.com/docs/2020/2020-11-13-vm-networks/
  * describes networking in kvm and illustrates some relevant commands

#### Virtio

"Para-virtualized" devices.

#### useful commands

* `virsh`
  * `virsh net-edit default`
  * `virsh net-dhcp-leases default`
  * `virsh net-destroy default && virsh net-start default`
* `tcpdump`
  * capture packets on an interface
  * `tcpdump -i virbr0`
* `dhclient`
  * get a new ip via dhcp