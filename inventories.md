# Working with Ansible Inventories

### Inventory-Hosts-Groups

- create a file called inventory
- to add a group, just add a line between [] -> [media]
- to add servers or host, justt a line under the group -> host1
e.g.

[group1]
host1
host2
host3

- to define group variables, create a folder, e.g. group_vars, and inside create a file for each group called group1
Inside this file called same as the group on inventory, add the variables:
e.g.
media_content: /var/media/content/
media_index: /opt/media/mediaIndex

For ansible documentation, the steps are 1. create a group on inventory | 2. Add variables for that group

- To add a variable to one host specific, not a group
1. Create a new directory -> hosts_vars
2. Inside create a file what match host name -> touch web1
3. Inside web1, add the variables you want
