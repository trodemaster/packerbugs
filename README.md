# packerbugs
To reproduce the issue https://github.com/hashicorp/packer/issues/6947 see below

This packer template has two nearly identical builders. 
*qemu-legacy* uses the qemu args "-usbdevice tablet" which works on 2.x qemu but has issues on 3.x
*qemu-new* uses the newer syntax "-usb -device usb-tablet" which works well for qemu 3.x but packer is having issues with multiple -device argument processing

Set some env vars to get logging setup and such
source ./envvars.sh

Both templates need the following options in addition to the usb tablet device configured. 
"disk_interface": "virtio-scsi"
"net_device": "virtio-net"

Build the legacy version
packer build -force -only qemu-legacy Ubuntu1804.json

This is what the qemu string looks like for qemu-legacy. Note the scsi devices are present. 
Executing /usr/local/bin/qemu-system-x86_64: []string{"-name", "Ubuntu1804.qcow2", "-m", "1024", "-netdev", "user,id=user.0,hostfwd=tcp::2917-:22", "-device", "virtio-scsi-pci,id=scsi0", "-device", "scsi-hd,bus=scsi0.0,drive=drive0", "-device", "virtio-net,netdev=user.0", "-drive", "if=none,file=VM/Ubuntu1804-qemu-legacy/Ubuntu1804.qcow2,id=drive0,cache=unsafe,discard=ignore,format=qcow2,detect-zeroes=off", "-vnc", "127.0.0.1:11", "-machine", "type=pc,accel=kvm", "-smp", "2", "-usbdevice", "tablet", "-cdrom", "/files/packer-itcloud/ISO/ubuntu-18.04-server-amd64.iso", "-boot", "once=d"}

This warning is also reported
Qemu stderr: qemu-system-x86_64: -usbdevice tablet: '-usbdevice' is deprecated, please use '-device usb-...' instead

Build the new version
packer build -force -only qemu-legacy Ubuntu1804.json

This is what the qemu string looks like for qemu-legacy

