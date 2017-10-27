% Configuring SSH for Virtualbox VM
% Ravikiran K.S.
% January 1, 2006

## SSH from Host to Virtualbox VM

1. Open Virtualbox App. Go to Preferences->Network->Host-only networks, click + to add vboxnet0
2. Select Ubuntu VM on VM list, go to Settings->Network->Adapter 2
    - select "Enable network adapter"
    - Attached to "Host-only adapter", name automatically chooses 'vboxnet0'
    - Edit vboxnet0, choose a different IP address range <range>.1 for vboxnet0 and
    - .100 for DHCP server, .101 to .254 for all interfaces on VM
3. Ensure "Enable IO APIC" is selected in Settings->System->Motherboard section of VM
4. Run given VM, for which settings were changed.
    - sudo vi /etc/network/interfaces
    - add below lines. Keep eth0 dynamic/dhcp for internet connections. Use eth1 for local connect
    - Save the configuration and reload the box.

[16/Mar/28 12:14:12|~]$ diff interfaces /etc/network/interfaces
3a4,15
> 
> # primary network interface adapter1, connects to internet. static doesn't work
> allow-hotplug eth0
> iface eth0 inet dhcp
> 
> # local host-only adapter 2. IP address should be higher than
> # IP address of vboxnet0. Host (MacOS) uses vboxnet0 IP addr
> # to communicate with Guest OS configured with IP addr below
> allow-hotplug eth1
> iface eth1 inet static
>     address 1.1.1.101
>     netmask 255.255.255.0

5. Now, vboxnet0 IP address would be configured on Host/Mac OS, and .101 would be configured on
eth1 interface of the guest OS.
6. Try pinging .1 from VM and .101 from Host shell. Both should ping.
NOTE: This wouldn't work if you are on any VPN connection

7. CLI commands to manage VirtualBox VMs: vbox.sh

8. Enable console server for Virtual Machine. So that you can connect to VM even when on VPN
    - Install VirtualBox Extension Pack
    - Configure RDP port for given VM as: VM Settings -> Display -> Remote Display
        - Tick "Enable Server"
        - Change "Server Port" to 5001
        - Do not Tick "Allow Multiple Connections"

9. Now after starting VM, you can do RDP at 127.0.0.1:5001 port with username/pass as usual.
    - This connection wouldn't break regardless whether you are connected to VPN or not.
