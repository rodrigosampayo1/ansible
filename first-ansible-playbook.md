#First playbook video

---

 - hosts: all

   become: yes

   tasks:

     - name: edit host file

       lineinfile:

         path: /etc/hosts

         line: "ansible.xyzcorp.com 169.168.0.1"

     - name: install elinks

       package:

         name: elinks

         state: latest

     - name: create audit user

       user:

         name: xyzcorp_audit

         state: present

     - name: update motd

       copy:

         src: /home/ansible/motd

         dest: /etc/motd

     - name: update issue

       copy:

         src: /home/ansible/issue

         dest: /etc/issue

### QUE CARAJO HACE?
- editar el archivo /etc/hosts
- instala elinks con PACKAGE, se puse usar YUM tambien. Si estas en debian, usar APT
- crea usuario xyzcorp_audit
- actualiza el archivo motd, toma en cuenta el archivo local del control,y lo copia en los workers
- actualiza el archivo issue, igual anterior

### Le agregue mas plays al playbook
#- hosts: network
  become: yes
  tasks:
    - name: install netcat
      yum:
        name: nmap-ncat
        state: latest
    - name: create network user
      user:
        name: xyzcorp_network
        state: present
-- Esta parte del playbook instala nmpa-ncat, y crea otro usuario


#- hosts: sysadmin
  become: yes
  tasks:
    - name: copy tarball
      copy:
        src: /home/ansible/scripts.tgz
        dest: /mnt/storage/

-- Esta parte del playbook copia un archivo del nodo control a los nodo host

