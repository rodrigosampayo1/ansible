# Deploying Services using Ansible

### Skill Level Professional

password YK]^z6Y6
ip 3.237.238.134

- Esta variable se usa ara configurar el NFS exports, usamos un template exports.j2
{{ share_path }} *(rw)

- Se actualiza el /etc/exports
127.0.0.1 localhost {{ ansible_hostname }}
{{ nfs_ip }}  {{ nfs_hostname }}

### Playbook here ###
 hosts: nfs
  become: yes
  vars:
    share_path: /mnt/nfsroot
  tasks:
    - name: install nfs
      yum:
        name: nfs-utils
        state: latest
    - name: start and enable nfs-server
      service:
        name: nfs-server
        state: started
        enabled: yes
    - name: configure exports
      template:
        src: /home/ansible/exports.j2
        dest: /etc/exports
      notify: update nfs
  handlers:
    - name: update nfs exports
      command: exportfs -a
      listen: update nfs

- share_path: /mnt/nfsroot -> ESTO ES UNA DEFINICION DE VARIABLE
- En el template de exports.j2, se usa para configurar la carpeta por defecto para los nfs
- notify -> Es para NOTIFICAR al manejador, o handler
- handlers -> Es el manjeador de NFS, que usa el comando EXPORTFS -A. Ademas el handler esta en escucha del UPDATE NFS puesto en notify.
Entonces cada vez que modifiquemos el template, se estara ejecutando el EXPORTFS -a, lo cual actualiza la NFS EXPORT LIST

### Add these lines:

#### Variables magicas de Ansible
- Las variables magicas de ansible se usan para acceder a informacion de operadores de Ansible, por ejemplo version de python usada, hosts, grupos, etc.
Las mas comunes son:
- hostvars (podes acceder a variables definidas por cualquier host en cualquier parte del playbook),
- groups(Podes enumerar todos los hosts dentro de un grupo, porque contiene una lista de grupos)
- group_names, inventory_hostname,etc



   #- hosts: remote #El grupo que afecta es REMOTE
     become: yes #Aca usa esto porque necesita tener acceso root para modificar recursos
     vars: 
       nfs_ip: "{{ hostvars['nfs']['ansible_default_ipv4']['address'] }}"   #Es una reg exp, cualquer host con NFS o IP o ADDRESS y lo guarda en la variable NFS-> IP /24 ADDRESS
       nfs_hostname: "{{ hostvars['nfs']['ansible_hostname'] }}"	    # Es lo mismo pero cualquier host con NFS y el hostname de Ansible, USA VARIABLES MAGICAS-> IP NOMBRE
     vars_files:
       - /home/ansible/user-list.txt #Toma las lineas de este archivo y las toma como variables
     tasks:
       - name: configure hostsfile
         template:
           src: /home/ansible/etc.hosts.j2
           dest: /etc/hosts
       - name: get file status
         stat:
           path: /opt/user-agreement.txt
         register: filestat				#Con REGISTER crea la variable FILESTAT, en donde se guarda el status del file en PATH
       - name: debug info
         debug:
           var: filestat
       - name: create users
         user:
           name: "{{ item }}"				#Este tarea crea usuarios, mientras que el archivo FILESTAT EXISTA, y LOOPEA sobre los usuarios
         when:  filestat.stat.exists
         loop: "{{ users }}"
