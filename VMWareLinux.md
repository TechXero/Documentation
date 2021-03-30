>> Install VMware-Workstation <<
 
Workstation Key : ZF71R-DMX85-08DQY-8YMNC-PPHV8<br />
Player Pro Key  : FA1M0-89YE3-081TQ-AFNX9-NKUC0<br />
 
The AUR package vmware-workstation will install VMWare Workstation and the dkms modules needed to run it.<br />
 
Firs make sure to have your kernel headers installed with :<br />
 
> sudo pamac -S linux5x-headers (Where x represents version eg linux59-headers if you on 5.9.x Kernel)<br />
 
It can be installed with:<br />
 
> pamac build vmware-workstation<br />
 
After installing, either reboot, or load the required modules<br />
 
> modprobe -a vmw_vmci vmmon<br />
 
Start and enable services<br />
 
If you need the vm to have network access (add --enable) if you want any of the service to start at boot.<br />
 
> systemctl enable vmware-networks<br />
 
If you need to pass on USB device into the vm<br />
 
> systemctl enable vmware-usbarbitrator<br />
 
Installing Manjaro under VMWare<br />
 
There are no special requirements to installing Manjaro on VMWare. open-vmware-tools is pre-installed. It should "just work"<br />
 
Optional Steps :<br />
 
Start and enable services<br />
 
There are three services that can be optionally be enabled:<br />
 
vmware-networks.service: Provides network access inside VMs, most people will want this enabled<br />
vmware-usbarbitrator.service: Allows USB devices to be connected inside VMs<br />
vmware-hostd.service: Enables sharing of VMs on the network<br />
 
To start and enable these services use:<br />
 
> sudo systemctl enable --now vmware-networks.service<br />
> sudo systemctl enable --now vmware-usbarbitrator.service<br />
> sudo systemctl enable --now vmware-hostd.service<br />
