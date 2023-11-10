# Playbooks and Module
In Ansible, a playbook is a configuration file written in YAML format that defines a set of tasks, roles, and configurations to be applied to a group of remote servers. Playbooks are at the core of Ansible automation and serve as a way to describe how Ansible should perform various tasks and manage server configurations.

A playbook typically consists of the following elements:

1. Hosts: Specifies the target servers or hosts on which the tasks will be executed.

2. Tasks: A list of actions or operations to be performed on the target hosts. These can include tasks like installing software, copying files, starting services, and more.

3. Roles: Playbooks can include reusable roles, which are collections of tasks and configurations organized for specific purposes. Roles make it easier to structure and reuse code across different playbooks.

4. Variables: Playbooks can define variables that can be used to customize the tasks and configurations based on specific conditions or server attributes.

5. Handlers: Handlers are tasks that are executed only when specific events occur, usually in response to changes caused by other tasks in the playbook.
```
- host: websrvgrp
   tasks:
      - yum:
          name: http
          state: present
```

- Let's engage in an exercise. Execute the commands provided below in the Git Bash terminal on the Control Server.
  
```
ls
cp -r exercise4 exercise5
cd exercise5
ls
rm -rf index.html

vim web-db-playbook.yaml
---
- name: Webserver Setup
  hosts: webservers
  become: yes
  tasks:
    - name: Install httpd
      ansible.builtin.yum:
        name: httpd
        state: present

    - name: Start service
      ansible.builtin.service:
        name: httpd
        state: started
        enabled: yes

- name: DBserver Setup
  hosts: dbservers
  become: yes
  tasks:
    - name: Install mariadb-server
      ansible.builtin.yum:
        name: mariadb-server
        state: present

    - name: Start mariadb service
      ansible.builtin.service:
        name: mariadb
        state: started
        enabled: yes



- name: DBserver Setup
  hosts: dbservers
  become: yes
  tasks:
    - name: Install mariadb-server
      ansible.builtin.yum:
        name: mariadb-server
        state: present
  tasks:
    - name: Start mariadb service
      ansible.builtin.yum:
        name: mariadb-server
        state: started
        enabled: yes

:wq

"uninstall previously installed packages"
ansible webservers -m yum -a "name=httpd state=absent" -i inventory --become

"execute playbook"
ansible-playbook -i inventory web-db-playbook.yaml

```
OUTPUT PICTURE

- For debugging purposes, we can pass -v, -vv, or -vvv to analyze the output.
- 

```
ansible-playbook -i inventory web-db-playbook.yaml -v
ansible-playbook -i inventory web-db-playbook.yaml -vv
ansible-playbook -i inventory web-db-playbook.yaml -vvv

```
- we can use the following command to check the syntax error in the playbook
  
```
ansible-playbook -i inventory web-db-playbook --syntax-check

```
- Performing a dry run of the playbook before executing the actual playbook is considered a best practice.

```
ansible-playbook -i inventory web-db-playbook -C

```

## <a href="https://docs.ansible.com/ansible/latest/" target="_blank">Ansible Documentation for reference </a>
- <a href="https://docs.ansible.com/ansible/latest/collections/index_module.html" target="_blank">Index of all Modules </a> For instance, you can look up the "yum" module used in the exercise. By clicking on the module, you can navigate to the example section.

