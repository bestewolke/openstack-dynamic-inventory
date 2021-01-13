# openstack-dynamic-inventory
Example for the OpenStack Dynamic Inventory Plugin

## Prerequisites
Use Ansible 2.10

## Usage
Configure your OpenStack clouds in `clouds.yaml` and passwords in `secure.yaml` (look into the files for examples).

You can run the Inventory Plugin to test your setup:
```
ansible-inventory -i openstack.yaml --graph
```
`--graph` displays a nice inventory graph output  
`--list` displays all the host info as json

The inventory gets cached in the `dynamic_inventory` folder with a timeout of one hour.

***

You can use the Dynamic Inventory for your Playbook:
```
ansible-playbook -i openstack.yaml myplaybook.yaml
```
or an Ansible ad-hoc command:
```
ansible all -i openstack.yaml -m ansible.builtin.ping --ssh-common-args="-o StrictHostKeyChecking=no"
```

Read more about:  
[Ansible Inventory Plugins](https://docs.ansible.com/ansible/latest/plugins/inventory.html#inventory-plugins)  
[OpenStack Inventory Plugin](https://docs.ansible.com/ansible/latest/collections/openstack/cloud/openstack_inventory.html)
