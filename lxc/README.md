## Part 1: Installation

### Install LXD and Other Dependancies

```shell
sudo apt-get update
sudo apt-get -y upgrade
sudo apt install acl dnsmasq-base git golang liblxc1 lxc-dev libacl1-dev make pkg-config rsync squashfs-tools tar xz-utils
sudo apt-get install lxd zfsutils-linux bridge-utils
sudo apt install -t xenial-backports lxd lxd-client
sudo apt install lxc-utils
dpkg -l | grep lxd
apt policy lxd lxd-client # Check the version
# BACKEND is one of btrfs, ceph, dir, lvm or zfs.
sudo apt install lvm2 thin-provisioning-tools # Install storage backends for LXD
sudo apt install btrfs-tools # Install storage backends for LXD
sudo apt install curl gettext jq sqlite3 uuid-runtime bzr # To run the testsuite
sudo apt install zfsutils-linux # Install ZFS
sudo apt install zfs-initramfs # Installing ZFS
```

### Initialize LXD

```shell
sudo lxd init # Initialize LXD
sudo lxc config show # Check the configuration
```
### Profile

```shell
lxc profile list
lxc profile show default
lxc profile device get default eth0 nictype
lxc profile device list default
lxc profile copy  default testprofile
```

### Create containers

```shell
sudo lxc launch ubuntu:16.04 CONTAINER-NAME
sudo lxc stop CONTAINER-NAME
sudo lxc delete CONTAINER-NAME
sudo lxc list # List containers
sudo ls -l /var/lib/lxd/containers/ # List containers
sudo ls -l /var/lib/lxd/containers/CONTAINER_NAME/ # List container CONTAINER_NAME
```

```shell
# Create VMs
for i in `seq 1 4`;
do
sudo lxc launch ubuntu:16.04 test0$i -p default
done
lxc list
```

```shell
lxc list |  grep -oP "test[0-9]{2}"  | xargs -I@ lxc stop @
lxc list |  grep -oP "test[0-9]{2}"  | xargs -I@ lxc delete @
```

### Clean up the storage backend

```shell
lxd init
lxc stop CONTAINER_NAME
lxc delete CONTAINER_NAME
```

```shell
lxc image list # List the cached images
lxc image delete FINGERPRINT # Cleaning up the images
```


### Clearing up the ZFS storage

```shell
lxc config show
lxc config unset storage.zfs_pool_name
lxc config show
sudo zpool list
sudo zfs list
sudo zpool destroy POOL-NAME
sudo zpool list
sudo zfs list
```




## Part 2: Configuration

```shell
# ifup: brings a network interface up, making it available to transmit and receive data.
# ifdown: takes a network interface down, placing it in a state where it cannot transmit or receive data.
# ifquery: parses the configuration of a network interface, allowing you to get answers to questions about how it is currently configured.
cat  /etc/network/interfaces # The file consists of zero or  more  "iface",  "mapping",  "auto",  "allow-"  and  "source" stanzas
```

```shell
sudo lxc exec test01 -- /bin/bash
sudo lxc exec CONTAINER-NAME -- su ubuntu
sudo /sbin/shutdown -r now
```




```shell
# Bridges can be controlled with brctl
brctl addbr lxdbr1 # Add bridge
ip link set lxdbr1 down # Bring the bridge down
brctl delbr lxdbr1 # delete bridge
brctl show # show a list of bridges
```



```shell
sudo lxc network create testbr0
sudo lsusb
sudo lxc config device add CONTAINER-NAME Realtek usb vendorid=0bda productid=0811
lxc config device add test01 enp63s0 nic name=eth1 nictype=bridged parent=lxcbr0
lxc config device add CONTAINER-NAME enp63s0 nic name=eth1 nictype=bridged parent=lxdbr0
lxc config device add CONTAINER-NAME enp63s0 nic name=eth1 nictype=physical parent=lxdbr0
lxc config device add CONTAINER-NAME enp63s0 none
lxc config device remove CONTAINER-NAME enp63s0
sudo lxc exec CONTAINER-NAME -- lsusb
lxc network attach lxdbr2 test02 default wlxe84e06491250

```


brctl show # Show all instances of the Ethernet Bridge
brctl showmacs lxdbr0 # Display a list of learned MAC addresses for lxdbr0
ip addr show lxdbr0 # Check ip address assinged to lxdbr0
nmcli connection show --active # Get current network config

lxc-device -n test01 add wlxe84e0649142f
lxc-device -n test02 add wlxe84e06491250

lxc profile copy default lanprofile

lxc profile device add bridgeprofile root disk path=/var/lib/lxd/disks/default.img pool=defaulte
lxc profile device add bridgeprofile root disk path=/var/lib/lxd/disks/default.img pool=default

```shell
cat <<EOF | lxc profile edit bridgeprofile
description: Bridged networking LXD profile
devices:
  eth0:
    name: eth0
    nictype: bridged
    parent: lxdbr0
    type: nic
EOF

lxc profile show bridgeprofile
```

lxc profile device set lanprofile eth0 nictype physical
lxc profile device set lanprofile eth0 parent wlxe84e06491250



lxc profile assign test01 default,bridgeprofile
lxc exec test01 -- systemctl restart networking.service


```shell
lspci | egrep -i --color 'network|ethernet'
ip link show
cat /proc/net/dev
netstat -i
ifconfig ETH-NAME
iwconfig wlxe84e06491250
ethtool enp63s0
```

```shell
cat /sys/class/net/enp63s0/device/vendor
cat /sys/class/net/enp63s0/device/device
cat /sys/class/net/enp63s0/device/uevent
sudo lxc exec CONTAINER-NAME -- ifconfig
ls -al /sys/class/net/ # List interfaces
ls -al /sys/class/net/enp63s0/

```



```shell
lxc profile list
lxc profile show default
lxc config edit CONTAINER-NAME
lxc config set <container> <key> <value>
lxc config  device add <container> <name> <type> [key=value]...
lxc profile device add <profile> <name> <type> [key=value]...
```

Device types: none, nic, disk, unix-char, unix-block, usb, gpu, infiniband, proxy



sudo lxc config device add test01 wisetiger4 usb vendorid=0bda productid=0811
sudo lxc config device add test02 wisetiger6 usb vendorid=0bda productid=0811

### Create bridge
```shell
lxc network list #  List available networks
lxc network create BRIDGE-NAME
lxc network create BRIDGE-NAME ipv6.address=none ipv4.address=10.0.3.1/24 ipv4.nat=true
lxc network show BRIDGE-NAME
lxc network attach-profile BRIDGE-NAME default ETH-NAME
lxc network attach BRIDGE-NAME CONTAINER-NAME default ETH-NAME
lxc network delete BRIDGE-NAME # Remove network bridge
```

lxc config device add test01 wisetiger4 usb vendorid=0bda productid=0811
lxc config device add test02 wisetiger6 usb vendorid=0bda productid=0811
lxc-device -n test01 add wlxe84e0649142f

nmcli con show
nmcli con delete bridge-lxdbr2
nmcli con delete bridge-slave-wlxe84e06491250
nmcli con add type bridge ifname lxdbr2
nmcli con add type bridge-slave ifname wlxe84e06491250 master lxdbr2
nmcli con up lxdbr2
nmcli connection show --active


```shell
sudo ip link add name lxdbr0 type bridge
sudo ip link set lxdbr0 up
sudo ip link add name lxdbr0 type bridge
sudo ip link delete lxdbr0
```

```shell
sudo apt install libvirt-bin lxc
sudo dpkg -s libvirt-bin | grep '^Version:'
```
service networking restart
ethtool enp63s0
mii-tool enp63s0

### Monitoring Container Utilization

```shell
sudo apt install virt-top # install virt-top
sudo virt-top -c lxc:/// # Open a "top" like interface to show container activities
```


### Virtual Machine Manager GUI

```shell
sudo apt install virt-manager
sudo virt-manager
```
```shell
service lxd start
service lxd stop
```

```shell
PASSWORD="Passw0rd"
#networking
lxd init --auto --network-address 0.0.0.0 --network-port 8443 --trust-password=$PASSWORD
lxc network create internal
lxc network attach-profile internal default eth0
```



```shell
sudo iw reg get
sudo iw reg set BZ
sudo iw reg get
sudo iw list
sudo iw dev
sudo ip link set wlxe84e0649142f down
sudo iw dev wlxe84e0649142f set txpower fixed 30mBm
sudo iw wlxe84e0649142f set monitor control # if monitor mode needed
sudo ip link set wlxe84e0649142f up

sudo iw dev wlxe84e0649142f get power_save
sudo iw dev wlxe84e0649142f set power_save off
iw wlan0 set power_save off
sudo iw dev wlxe84e0649142f link
iw wlan0 connect foo 2432
sudo iw wlxe84e0649142f connect ETS-Campus

```



```shell
# install the WT driver
mkdir -p /tmp/t4u
cd /tmp/t4u
wget https://github.com/abperiasamy/rtl8812AU_8821AU_linux/archive/master.zip
# Unzip
unzip master.zip
cd rtl8812AU_8821AU_linux-master
# Compile and install from source
make
sudo make install
# Load module
sudo modprobe -a rtl8812au
```

```shell
nmcli -f chan,ssid,bssid,signal,security dev wifi
sudo nmcli dev status
sudo service network-manager restart
sudo ifconfig wlxe84e0649142f
nmcli dev wifi connect LACIMERJM password eVw#6BXPnt*
sudo nmcli d wifi connect 50:C7:BF:9D:86:F1 password eVw#6BXPnt*
sudo iwconfig wlxe84e0649142f essid LACIMERJM key s:eVw#6BXPnt*
dhclient wlxe84e0649142f
```


echo "hello root"
sudo iw reg get
sudo ip link set wlxe84e0649142f down
sleep 3
echo "Boosting Tx Power To 30 Fixed"
sudo iw dev wlxe84e0649142f set txpower fixed 10
sleep 3
echo "starting wlxe84e0649142f"
sudo ip link set wlxe84e0649142f up
sleep 2
echo "Checking wlxe84e0649142f TxPower"
sudo iw dev
sleep 3
echo "Checking Regulatory Domain"
sudo iw reg get
sleep 2
echo "Good Luck"


wifi-status
iwconfig wlp2s0; ifconfig wlp2s0; route -n; echo; (grep wlp2s0: /var/log/syslog | tail -n 10 | sort -r); echo; ping -I wlp2s0 -c 1 8.8.8.8
