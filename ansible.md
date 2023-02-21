# Ad-Hoc Ansible Commands


### Create users

- ansible dbsystems -b -m user -a "name=consultante"

dbsystem -> a host group. If we use an INVENTORY, we need to add "-i inventory"
-b BECOME-> because we're using commands what need root access
-m MODULE-> Because we're a calling a MODULE. In this case, User module, because we need to create a user
-a ARGUMENT-> to provide the name to User module 

### Create ssh directory for a user

- ansible dbsystems -b -m file -a "path=/home/consultant/.ssh state=directory owner=consultant group=consultant mode=0755"
in this case we use another module called USER

### Copy each Host key to their respective folder. Each keys have created and save in /home/ansible/keys
- ansible dbsystems -b -m copy -a "src=/home/ansible/keys/consultant/authorized_keys dest=/home/consultant/.ssh/authorized_keys mode=0600 owner=consultant group=consultant"
in this case, we use the COPY module, to copy the keys in the Source place to the destiny place, with mode-owner-group values

### Enable auditd on all hosts

- ansible all -b -m service -a "name=auditd state=started enabled=yes"
in this case, we use all because it is the same command for all hosts


