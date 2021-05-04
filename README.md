# openstack-dynamic-inventory
Example for the OpenStack Dynamic Inventory Plugin

## Prerequisites
- Ansible 2.10
- openstacksdk

## Usage
Configure your OpenStack clouds in `clouds.yaml` and passwords in `secure.yaml` (look into the files for examples).

You can run the Inventory Plugin to test your setup:
```
ansible-inventory -i openstack.yaml --graph
```
`--graph` displays a nice inventory graph output  
`--list` displays all the host info as json

The inventory gets cached in the `dynamic_inventory` folder with a timeout of one hour.

#### Advanced Host Grouping
You can add tags to your OpenStack VMs and group them according to those tags.  
There are two grouping mechanisms:
- **keyed_groups**:  This will automatically analyze all tags & values and create corresponding inventory groups.  
The group names are auto-generated, you cannot change them. They consist of the prefix, the tag key and the value.  
For OpenStack tags the syntax is like this (choose a prefix to your liking):
  ```yaml
  keyed_groups:
    - key: openstack.tags
      prefix: tags
  ```
  Result:
  ```
  @all:
  |--@eu-nl:
  |  |--webserver-0001
  |  |--webserver-0002
  |  |--webserver-0003
  |  |--webserver-0004
  |  |--webserver-0005
  |  |--webserver-0006
  |  |--…
  |--@tags_env_production:
  |  |--webserver-0001
  |  |--webserver-0002
  |  |--webserver-0003
  |--@tags_env_staging:
  |  |--webserver-0004
  |  |--webserver-0005
  |  |--webserver-0006
  |--@ungrouped:
  ```
- **groups**: You can group your hosts with Jinja2 conditionals. With this you can also set your own group names.  
For OpenStack tags the syntax is like this (the example creates two groups: staging & production, and adds hosts according to their tag key/value pairs):
  ```yaml
  groups:
    staging: "'env=staging' in (openstack.tags|list)"
    production: "'env=production' in (openstack.tags|list)"
  ```
  Result:
  ```
  @all:
  |--@eu-nl:
  |  |--webserver-0001
  |  |--webserver-0002
  |  |--webserver-0003
  |  |--webserver-0004
  |  |--webserver-0005
  |  |--webserver-0006
  |  |--…
  |--@production:
  |  |--webserver-0001
  |  |--webserver-0002
  |  |--webserver-0003
  |--@staging:
  |  |--webserver-0004
  |  |--webserver-0005
  |  |--webserver-0006
  |--@ungrouped:

  ```

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
