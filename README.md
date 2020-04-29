# OpenStack ovs Ansible
This ansible script will provisioning OpenStack with OpenvSwitch Enabled.

### Tested
- OpenStack Queens
- 1 controller 3 compute

### Limitation
- Only support single controller
```
zu-ovs-controller0
Interface Management (eth0) : 10.100.100.200
Interface Data       (eth1) : 10.101.101.200
Interface External   (eth2) : no ip address

zu-ovs-compute0
Interface Management (eth0) : 10.100.100.210
Interface Data       (eth1) : 10.101.101.210

zu-ovs-compute1
Interface Management (eth0) : 10.100.100.211
Interface Data       (eth1) : 10.101.101.211

zu-ovs-compute2
Interface Management (eth0) : 10.100.100.212
Interface Data       (eth1) : 10.101.101.212
```

### Installation
- Setup hosts mapping
```
nano /etc/hosts

10.100.100.200 zu-ovs-controller0
10.100.100.203 zu-ovs-compute0
10.100.100.204 zu-ovs-compute1
10.100.100.205 zu-ovs-compute2
```
- Setup and copy key from bootstrap node to all nodes
```
yum install sshpass
ssh-keygen

sshpass -p "rahasia" ssh-copy-id -o StrictHostKeyChecking=no root@zu-ovs-controller0
sshpass -p "rahasia" ssh-copy-id -o StrictHostKeyChecking=no root@zu-ovs-compute0
sshpass -p "rahasia" ssh-copy-id -o StrictHostKeyChecking=no root@zu-ovs-compute1
sshpass -p "rahasia" ssh-copy-id -o StrictHostKeyChecking=no root@zu-ovs-compute2
```
- Install ansible in bootstrap node
```
yum -y update
yum -y install epel-release
yum -y install nano git python python-pip

pip install ansible==2.5.5
```
-  Disable ansible host key checking
```
nano ~/.ansible.cfg

[defaults]
host_key_checking = False
```
- Clone Repository
```
git clone https://github.com/zufardhiyaulhaq/openstack-ovs-ansible.git
```
- Edit value
```
group_vars/all.yml
hosts/hosts
```
- Run Ansible
```
ansible-playbook main.yml -i hosts/hosts
```
