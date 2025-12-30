![header](https://github.com/nbonnand/ovs-toolbox/blob/master/src/wiki/header.jpg)

# ovs-toolbox
ovs-toolbox.py is a graphical user interface for Open vSwitch (OvS).

![header](https://github.com/nbonnand/ovs-toolbox/blob/master/src/wiki/header2.jpg)

OvS bridges managed by this GUI, do not necessarily need to be local to your host. 
(They can be located on AWS, GCP, etc.. and managed remotely by this GUI via ssh/paramiko/sudo ) 

In addition, this GUI will help you with docker and KVM ecosystems (for simple tasks like connecting containers and VMs to OvS).

OvS related settings that you can manage with this GUI:
- bridge creation
- port/interface creation ( vlan, interface types, etc..)
- port statistics
- ingress policy
- mirroring
- bonding
- RSTP, STP
- multicast
- flows (netflow,sflow,ipfix)
- queues and QOS
- OpenFlow flows
- OpenFlow groups
- various OvS databases (controller, manager, Open_vSwitch, ssl )

Docker related settings that you can manage with this GUI:
- docker files creation
- docker image creation, docker image build
- docker run, stop and rm
- docker inspect
- docker containers network parameters and automatic connection to selected OvS through one or multiple network interfaces.

KVM related things you can do with this GUI:
- virt-install settings
- KVM network parameters and automatic connection to selected OvS

iproute related things you can do with this GUI:
- interface creation (dummy, tap, tun, veth pair )
- MTU setting
- ip address setting

Plotnetcfg
- live image of network diagram generated thanks to [Plotnetcfg](https://github.com/jbenc/plotnetcfg) and [Graphviz](https://www.graphviz.org/)

For documentation, read [ovs-toolbox wiki](https://github.com/nbonnand/ovs-toolbox/wiki)

---

&copy; 2018 Nicolas Bonnand, Licensed under the Apache License, Version 2.0



# Setup OVS

```shell

sudo apt install openvswitch-common openvswitch-switch python3-openvswitch

sudo ovs-vsctl = vsctl

vsctl --version
vsctl show

# add a bridge <br0>
vsctl add-br <br-name>

# add a veth pair <ethx, ethy>
sudo ip link add <if-name> type veth peer name <if-name>

# connect veth end to bridge
sudo ovs-vsctl add-port <br-name> <if-name>

# assign ip to bridge (gateway)
sudo ip a add 192.168.0.1/24 dev <br-name>

# bring up bridge or if
ip l set <if-name> up
ip l set <br-name> up


# connect vm to bridge
> Virtual Network > Network-Source = Bridge device ...
> device name: <br-name>
>> XML
>> <model type="virtio"/>
>> <virtualport type="openvswitch"/>

 
# connect docker
docker run -it --name mybox --rm --network none busybox
sudo ovs-docker add-port <br-name> <if-name> mybox --ipaddress="..../24" --gateway="...1"

# patch switch
vsctl add-port <br-name1> P1
vsctl add-port <br-name2> P2
vsctl set Interface P1 type=patch option:peer=P2
vsctl set Interface P2 type=patch option:peer=P1


# see flows
sudo ovs-ofctl dump-flows <br-name>

```

```
0) Prerequisites:

ovs-toolbox is a python3 software with a Qt5 interface.
It depends on:
- python3
- python3-Qt5
- lxml
- paramiko
- plotnetcfg
- Graphviz


1) INSTALLATION ON FEDORA Linux

1.1) Install all dependencies

dnf install python3 python3-Qt5 plotnetcfg graphviz
pip3 install lxml
pip3 install paramiko


1.2) Download ovs_toolbox

git clone https://github.com/nbonnand/ovs-toolbox.git

1.3) ovs-toolbox.py is an all-in-one single file software, you can basically install it where you want, however a good place is in /usr/local/bin.

cd ovs_toolbox/
./install_ovstoolbox.sh




1.4) Run it !

ovs-toolbox.py



2) Installation on WINDOWS 


2.0) IMPORTANT NOTICE: ovs-toolbox will work with known limitations

ovs-toolbox sofware was develop under and for linux platform. I am not familiar of Windows.
Despite I have tried to avoid linux python specific features in the code as much as I could, some features will not be working on windows platform.

2.1) Install Python3

Download it from https://www.python.org/downloads/windows/ and follow instructions

2.2) Install PyQt5
Follow instructions from: https://riverbankcomputing.com/software/pyqt/download5

2.3) Install paramiko and lxml

pip3 install lxml
pip3 install paramiko

2.4) Run it !

From a terminal launch the following command:
ovs-toolbox.py
```