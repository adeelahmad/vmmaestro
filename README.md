vmmaestro
=========

Thsi tiny shell script is for controlling KVM. There is the first note that this script NEVER managements VMs in all senses. It has no fail over mechanism including live migration, no beautiful GUI. You can't see any statistical information. In addition, it can't even create an new VM disk image.

vmmaestro (:D) is a simple command line tool to start, shutdown VMs and help to connect to its screen from your client PC.    

----
## Instllation    
### Step 1
Clone this repogitory.    
### Step 2
* Copy vmmaestor, which is a shell script, to anywhere you'd like, i.e. /usr/local/bin/
* Create ```/etc/vmmaestro``` directory.
    
### Step 3
If you use SPiCE and TLS, place 3 files into ```/etc/vmmaestro/ssl```.
* A PEM formated file merged CA Root certificate with CA intermediate certificate. It must be named ```ca-cert.pem```.
* A server certificate issued by CA. It must be named ```server-ca.pem```.
* A private key file. It must be named ```server-key.pem```.

### Step 4
Install ```sudo```, ```sasl2-bin```, ```brctl``` and ```qemu-kvm``` packages.

### Step 5
* If not created ```kvm``` user or other, add ```kvm``` user account.
* Add ```kvm``` user to /etc/sudoers (this repository includes a sample settings).

#### Step 6
* Create volume group and lvm partitions for VM as you prefer.

#### Step 7
* Add TUN/tap kernel module, ```tun``` to the system. (execute '```modprobe tun```')

#### Step 8
* Add bridge interface per NIC. Recommend to name a bridge like '```br0```', '```br1```'...

#### Step 9
* Create a sasl configuration file for QEMU (this repository includes a sample config file).
* Add user and password to the sasl database by using ```saslpasswd2```.    
	This is required to access VNC/SPiCE Console.    

#### Step 10
* Create the global configuration file, ```/etc/vmmaestro/vmmaestro.conf``` by using ```vmmaestro.conf.sample```
* Create the VM specific configuration file in ```/etc/vmmaestro```. The file name must be the same name as VM and has an extension, '```.conf```'.
* You can put all settings in the VM specific configuration and leave blank the global configuration. However, I recommend to put as many common settings as possible in the global configuration file. When the same item in both file, **the one put into the VM specific configuration is given priority**.

#### Step 11
* Type ```vmmaestro start vm-name``` and enjoy!

----
## Reference    
#### vmmaestro start [VM [VM [VM]...]]
start VMs. This command can boot multiple VMs.

#### vmmaestro shutdown [VM [VM [VM]...]]
shutdown VMs. When entering this command, VMs will do shutdown sequences.

#### vmmaestro stop [VM [VM [VM]...]]
stop VMs. When entering this command, VMs will terminate immediately
#### vmmaestro kill [VM [VM [VM]...]]
kill kvm process directly.

#### vmmaestro restart [VM [VM [VM]...]]
Reboot VMs.

#### vmmaestro status [VM [VM [VM]...]]
If VM is running, shows the message, "```VM name is running.```".
Otherwise shows the message, "```VM name is stopped.```"

#### vmmaestro console VM
Connect serial port and show local text screen. 

#### vmmaestro monitor VM
Connect KVM/QEMU monitor port.

----
## Hint    
* By default, VNC port is bind to localhost address (127.0.0.1). I recommend use ```SSH tunneling``` instead of changing bind address.
