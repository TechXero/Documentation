>> Install VMware-Workstation <<
 
Workstation Key : ZF71R-DMX85-08DQY-8YMNC-PPHV8
Player Pro Key  : FA1M0-89YE3-081TQ-AFNX9-NKUC0
 
The AUR package vmware-workstation will install VMWare Workstation and the dkms modules needed to run it.
 
Firs make sure to have your kernel headers installed with :
 
> sudo pamac -S linux5x-headers (Where x represents version eg linux59-headers if you on 5.9.x Kernel)
 
It can be installed with:
 
> pamac build vmware-workstation
 
After installing, either reboot, or load the required modules
 
> modprobe -a vmw_vmci vmmon
 
Start and enable services
 
If you need the vm to have network access (add --enable) if you want any of the service to start at boot.
 
> systemctl enable vmware-networks
 
If you need to pass on USB device into the vm
 
> systemctl enable vmware-usbarbitrator
 
Installing Manjaro under VMWare
 
There are no special requirements to installing Manjaro on VMWare. open-vmware-tools is pre-installed. It should "just work"
 
Optional Steps :
 
Start and enable services
 
There are three services that can be optionally be enabled:
 
vmware-networks.service: Provides network access inside VMs, most people will want this enabled
vmware-usbarbitrator.service: Allows USB devices to be connected inside VMs
vmware-hostd.service: Enables sharing of VMs on the network
 
To start and enable these services use:
 
> sudo systemctl enable --now vmware-networks.service
> sudo systemctl enable --now vmware-usbarbitrator.service
> sudo systemctl enable --now vmware-hostd.service
