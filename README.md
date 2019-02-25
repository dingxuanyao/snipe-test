# snipe-test
Building a Snipe IT asset management service on CentOS 7 VirtualBox


Settings up Guest Additions

Root user
$sudo su

$yum update kernel*
$reboot

Click Devices > Install Guest Additions… on VirtualBox

Mount VirtualBox Guest Additions device

$mkdir /media/VirtualBoxGuestAdditions
$mount -r /dev/cdrom /media/VirtualBoxGuestAdditions

On CentOS/Red Hat (RHEL) 7 EPEL repo is needed

$rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

Install following packages

$yum install gcc kernel-devel kernel-headers dkms make bzip2 perl

#Add KERN_DIR environment variable

$KERN_DIR=/usr/src/kernels/`uname -r`/build
$export KERN_DIR

