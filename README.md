# ansible-win-phoronix

This is a role for installing phoronix under windows.

## Requirements


  * KVM or similar
  * Ansible (2.8+?)
  * Compatible version of winrm for ansible

## Quickstart

### Download/run initial image

```
# Create and go into directory
mkdir MSEdge.Win10
cd MSEdge.Win10

# Download from https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/
wget https://az792536.vo.msecnd.net/vms/VMBuild_20190311/VirtualBox/MSEdge/MSEdge.Win10.VirtualBox.zip

# Unzip
unzip ../MSEdge.Win10.VirtualBox.zip

# Unpack
mv "MSEdge - Win10.ova" MSEdge-Win10.tar
tar xaf MSEdge-Win10.tar

# Convert to QCOW2
qemu-img convert "MSEdge - Win10-disk001.vmdk" -O qcow2 -S 4k -c MSEdge-Win10-disk001.qcow2

# Start under KVM with port redirection
qemu-system-x86_64 -enable-kvm -smp cpus=2 -m 2048M -net nic -net user,hostfwd=tcp::5985-:5985  /var/lib/kvm/MSEdge-Win10-disk001.working.qcow2
```

### Ubuntu Focal Packages

  * ansible
  * python3-winrm

### Manual setup in Windows VM

  * Log in using default credential IEUser / Passw0rd!
  * Click on network connection
  * Click on "network and internet settings"
  * Click on "change connection properties"
  * Select "private" not "public"

Possible automation methods:

  * Edit existing network registry setting to point to KVM adapter using
    reged from package chntpw
  * Edit the registry to allow ACPI shutdown without login before booting
    the machine for the first time (?), issue an ACPI shutdown from KVM
    after some amount of uptime, then edit the resulting network registry

References:

  * <https://answers.microsoft.com/en-us/insider/forum/all/how-to-change-private-network-to-public-network-in/60cd2c6b-0601-489a-ac42-8f1bafed49d3>
  * <https://www.oueta.com/microsoft/view-network-adapter-info-and-change-mac-address-on-windows-7-8-10/>
