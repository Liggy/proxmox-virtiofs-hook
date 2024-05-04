# Virtiofsd hook for Proxmox
Proxmox does not yet natively support adding filesystems to VM via virtiofsd. Instead you can use a script to configure the virtual machine and execute virtiofsd when the VM starts.
## Preparation
* Download the hook script and the config file to a snippet directory within your configured storage
* Make the hook script executable (chmod +x viofshook)
* Adjust the config file to include the directories you want to attach to your VM. A sample file is provided. Remove the sample extension and adjust according to your needs
## VM setup
Execute

    qm set {VMID} --hookscript {STORAGE}:snippets/viofshook

    viofshook {VMID} install
and replace `{VMID}` with the ID of your virtual machine and `{STORAGE}` with the name of your storage (e.g. local)

This enures that the virtiofs daemon is executed when the VM starts and that required parameters are added to the VM config. The second step cannot happen when the script is called from PVE, as the VM configuration is locked at this stage. You will also have to repeat the second command when changing memory size or shares for the VM.

Once your VM is running, you can mount your shares.
## Config file syntax
    vmid:tag1=directory1,tag2=directory2, ...
## Caveats
* This script will override all `args:` settings. Any manual changes will be overwritten when running the install routine
* Online migrations are not yet supported.