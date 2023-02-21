# Working Ansible templates, variables and facts

### FACTS
#### Variables related to remote systems are called facts. With facts, you can use the behavior or state of one system as configuration on other systems. For example, you can use the IP address of one system as a configuration value on another system. Variables related to Ansible are called magic variables.
Link
- https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html
e.g. ansible_default_ipv4.address



### VARIABLES
#### Ansible uses variables to manage differences between systems. With Ansible, you can execute tasks and playbooks on multiple different systems with a single command. To represent the variations among those different systems, you can create variables with standard YAML syntax, including lists and dictionaries. You can define these variables in your playbooks, in your inventory, in re-usable files or roles, or at the command line. You can also create variables during a playbook run by registering the return value or values of a task as a new variable.
Link
- https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html
e.g.
- remote_install_path: /opt/my_app_config


## Ejercicio, deploy en todos los servidores del inventario una especia de sudoers
- %sysops {{ ansible_default_ipv4.address }} = (ALL) ALL
Esto es el grupo sysops le asigna un FACT, asi como una funcion de ansible propia, y le da permisos de sudoers, como el archivo que tiene en sudo visudo
Le da a los usuarios del grupo sysops, la habilidad de correr comandos como ROOT para cada local system por IP address

- Hosts_Alias WEBSERVERS = {{ groups['web']|join(' ') }}
- Hosts_Alias DBSERVERS = {{ groups['database']|join(' ') }}
Para crear sudo alias para los hosts basado en el inventario, usando groups[] y usa join(' ') para crear un separador de lista
Resumido, crea una lista de los los servidores en ese group del inventario

- %httpd WEBSERVERS = /bin/su - webuser
- %dba DBSERVERS = /bin/su - dbuser
Estas dos lineas, le dan a los usuarios en el grupo httpd la HABILIDAD de "sudo su - webuser" en los hosts de WEBSERVERS
Lo mismo, los usuarios del grupo dba pueden hacer "sudo su - dbuser" en los host de DBSERVERS

### Template final
%sysops 10.0.1.166 = (ALL) ALL
Host_Alias WEBSERVERS = node1
Host_Alias DBSERVERS = node2
%httpd WEBSERVERS = /bin/su - webuser
%dba DBSERVERS = /bin/su - dbuser

### Despues del template anterior, creamos un playbook, llamado security.yml
Validar siempre el archivo que es va a colocar en sudores.d, porque si hay algo mal en la syntaxis, el sudo remote se rompe y fuiste
- The file must be validated using /sbin/visudo -cf before deployment.

cat security.yml
---
- hosts: all
  become: yes
  tasks:
  - name: deploy sudo template
    template:
      src: /home/ansible/hardened.j2
      dest: /etc/sudoers.d/hardened
      validate: /sbin/visudo -cf %s

#### Final command:
- ansible-playbook security.yml
