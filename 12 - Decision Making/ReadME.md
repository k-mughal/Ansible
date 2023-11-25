<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/c3b897d1-af7f-493f-b3e7-21862550cafd">
</p>

- We will commence server provisioning, simultaneously delving into the fascinating functionalities of Ansible playbooks. Using the NTP service as an illustrative example, we will configure this service across various operating systems, including Ubuntu. The setup will involve creating users and groups, as well as modifying server configuration files.

- Throughout this process, our focus will extend to understanding decision-making within the playbook, incorporating loops, utilizing templates for dynamic configurations, implementing handlers, and exploring Ansible rules. While executing these tasks, we will not only acquire new insights but also apply the features we have previously learned.

- It's worth noting that our emphasis isn't solely on the NTP service; rather, we aim to provide a general approach applicable to any service or server provisioning task. Once the playbooks are comprehensive, we will explore the concept of roles and optimal strategies for their utilization. Lets login to host server and execute the commands below.

```
cd ansible-prjs
cp -r exercise9 exercise10
rm -rf print_facts.yaml vars_precedence.yaml
ansible -m ping all
vim provisioning.yaml

---
- name: Provisioning Servers
  hosts: all
  become: yes
  tasks:
   - name: Install ntp agent on centOS
     yum:
       name: chrony
       state: present
     when: ansible_distribution == "CentOS"

   - name: Install ntp agent on ubuntu
     apt:
       name: ntp
       state: present
       update_cache: yes   # --> apt-update
     when: ansible_distribution == "Ubuntu"

   - name: Start service on CentOS
     service:
       name: chrony
       state: started
       enabled: yes
     when: ansible_distribution == "CentOS"

   - name: Start service on ubuntu
     service:
       name: ntp
       state: started
       enabled: yes
     when: ansible_distribution == "Ubuntu"


:wq:

ansible-playbook provisioning.yaml
```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/7223749d-50eb-4467-8b6b-73fea28093d9">
</p>



- Few other conditional examples from   **<a href="https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_conditionals.html" target="_blank">**Ansible documentation.**  </a>** 
   <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/283ae815-d843-45ef-b4dc-e306484229de">
</p>

