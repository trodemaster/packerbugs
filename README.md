# packerbugs
To reproduce the issue https://github.com/hashicorp/packer/issues/6947 see below

This packer template has two nearly identical builders. 
*qemu-legacy* uses the qemu args "-usbdevice tablet" which works on 2.x qemu but has issues on 3.x
*qemu-new* uses the newer syntax "-usb -device usb-tablet" which works well for qemu 3.x but packer is having issues with multiple -device argument processing
