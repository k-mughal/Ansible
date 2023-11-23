- n Ansible, a loop is a construct that allows you to iterate over a set of values and perform a task or a series of tasks for each item in that set. Loops are commonly used to avoid repetitive code and to apply the same configuration or operation to multiple items.

- There are different ways to implement loops in Ansible, and the most common one is by using the loop keyword along with the with_items parameter. However, starting from Ansible 2.5, the loop keyword is deprecated, and the recommended way is to use the loop attribute directly. Let start an exercise.

```
cd ansible-prjs
cp -r exercise10 exercise11
cd exercise11
vim provisioning.yaml
- name: Provisioning Servers
  hosts: all
  become: yes
  tasks:
   - name: Install ntp agent on centOS
     yum:
       name: "{{item}}"
       state: present
     when: ansible_distribution == "CentOS"
     loop:
       - chrony
       - wget
       - zip
       - unzip

   - name: Install ntp agent on ubuntu
     apt:
       name: ntp
       state: present
       update_cache: yes   # --> apt-update
     when: ansible_distribution == "Ubuntu"

   - name: Start service on CentOS
     service:
       name: chronyd
       state: started
       enabled: yes
     when: ansible_distribution == "CentOS"

   - name: Start service on ubuntu
     service:
       name: ntp
       state: started
       enabled: yes
     when: ansible_distribution == "Ubuntu"
:wq

```

 <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/c091d2f3-a089-46d7-b695-c91b335d942b">
</p>

```
ansible-playbook provisioning.yaml
```

 <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/a3fae52b-bfd1-4ed7-9e46-b6091149f273">
</p>

 Few more loops examples from   **<a href="https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html" target="_blank">**Ansible documentation.**  </a>** 