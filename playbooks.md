# Ansible Playbooks The basics

## Highview 
### Command -> ansible-playbook
### Modules -> services, yum, get-url
### It will be run an Apache server, enable and start

### Playbook
- every playbook file has a YML extension
- you can called it whatever you want
- starts with ---

e.g.
---
- hosts: web
  become: yes
  tasks:
    - name: httpd
      yum: name=httpd state=latest
    - name: start and enable httpd
    service: name=httpd state=started enabled=yes
    - name: retrieve website from repo
      get_url: url=http://repo.example.com/website.tgz dest=/tmp/website.tgz
    - name: install website
      unarchive: remote_src=yes src=/tmp/website.tgz dest=/var/www/html/

- To run this playbook ->  ansible-playbook -i inventory web.yml
- To validate this playbook -> curl node1/home.html
- or you can check the public ip, and you would see the Apache test page
