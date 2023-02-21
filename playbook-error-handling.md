# Ansible Playbooks 
## Error Handling
- Advanced error handling is one of the many strengths of Ansible. Software systems are seldom perfect, and that is an issue in this exercise. Students must configure an Ansible playbook to handle an unreliable connection. This skill is not only essential for practical Ansible use, but also an objective on the Red Hat Certified Ansible Specialist Exam.


### Playbook -> reports.yml
---
- hosts: localhost
  tasks:
    - name: download transactions_list
      block:
        - get_url:
            url: http://apps.l33t.com/transaction_list
            dest: /home/ansible/transaction_list
        - replace:
            path: /home/ansible/transaction_list
            regexp: "#BLANKLINE"
            replace: '\n'
        - debug: msg="File Downloaded"
      rescue:
        - debug: msg="133t.com appears to be down. Try again later."
      always:
        - debug: msg="Attempt Completed."


block -> actua como una llave para separar
get_url -> actua como wget, que toma lo de la rula, y lo guarda en dest
replace -> reemplace lo que encontro en PATH, usa una regular expresion. Si hay un espacio en blanco, pone un salto de linea
debug-> lo usa en block, para mostrar que se esta descargando

Si falla, va al bloque RESCUE, en donde va a mostrar un mensaje para avisar que fallo.

Ademas, agrega un mensaje, ya sea que falle o no, para que el usuario sepa que finalizo el proceso.


Para que todo esto? Si hay un error, por ejemplo que el sitio este caido, aparece lo siguiente:

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************************************
ok: [localhost]

TASK [get_url] *******************************************************************************************************************************************************************************************************************************
fatal: [localhost]: FAILED! => {"changed": false, "dest": "/home/ansible/transaction_list", "elapsed": 0, "msg": "Request failed: <urlopen error [Errno 111] Connection refused>", "url": "http://apps.l33t.com/transaction_list"}

TASK [debug] *********************************************************************************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "133t.com appears to be down. Try again later."
}

TASK [debug] *********************************************************************************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Attempt Completed."
}

PLAY RECAP ***********************************************************************************************************************************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0
