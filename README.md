# Ansible - Learning path

## Preview

- OS agnostic
- Ansible for Configuration. Not like Terraform (or Chef/Puppet/etc)
- agentless


## Links

- Basic bash -> https://tomsitcafe.com/2023/02/09/basic-bash-for-sysadmins-01-the-shell/
- Windows as Ansible control host in WSL2 -> https://tomsitcafe.com/2023/02/10/windows-as-ansible-control-host-in-wsl2/
- Getting started with Ansible for managing -> https://tomsitcafe.com/2023/02/13/getting-started-with-ansible-for-managing-our-personal-lab-ad-hoc-commands/
- Getting started with Ansible playbooks -> https://tomsitcafe.com/2023/02/14/getting-started-with-ansible-playbooks-more-steps-towards-devops/
- Creating an Ansible role from a playbook: modular & reusable code -> https://tomsitcafe.com/2023/02/15/creating-an-ansible-role-from-a-playbook-modular-reusable-code/
- Loops in the Ansible code -> https://tomsitcafe.com/2023/02/16/loops-in-the-ansible-code-the-basics-of-iteration/
- Conditional statements - making decisions in Ansible -> https://tomsitcafe.com/2023/02/17/conditional-statements-making-decisions-in-ansible-code/

## Videos
- Red Hat certified specialist in adanced automation: Ansible Best practices
https://learn.acloud.guru/course/red-hat-ex447-ansible-best-practices/dashboard

- Learn Ansible by Doing
https://learn.acloud.guru/course/e7251e47-a643-49f3-b096-28072dfa577b/dashboard

- Deep dive on Ansible Playbook
https://learn.acloud.guru/course/b0ad4cc4-1880-4ec7-a4fe-65a2f02b5eeb/overview


## Configuration
At least 2 VMs, 1 control node (the server) and workstation/s (host/s).
Both need an extra unique user. The workstation vm need a SSH key and The control has to have it.
On Control node, an inventory file with the workstations or hosts, using private IP or Hostname
On Control node, a playbook, what contains the tasks to deploy on workstations

- on control node -> sudo yum install ansible
- Create a new user and swtich users -> useradd Name 
				     -> passwd Name
				     -> sudo su - Name
- Give that user "Name"a sudoers permissions
- configug SSH preshared key using a new user					-> ssh-keygen
- Copy that keygen to every our workstation hosts, 1 by 1			-> ssh-copy-id NameWorkstation1  
- Create an inventory file, without extension, and add your hosts
- Connect to each workstation and give sudoers access to Control vm 		-> sudo visudo
- At the botom, add this line "ansible ALL=(ALL)       NOPASSWD: ALL", where ansible is the name of the control user
- To do a ping, add the -i command with the destinatories like this 		-> ansible -i inventory node1 -m ping

