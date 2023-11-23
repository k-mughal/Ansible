- In the preceding exercise, we installed NTP on the host machine,
 resulting in the creation of the NTP configuration file. Now, let's engage in some file operations by updating changes in NTP configuration file and restart the service. We can use the example from 
 **<a href="https://docs.ansible.com/ansible/2.8/modules/list_of_files_modules.html" target="_blank">**File Module -->**  </a>** 
**<a href="https://docs.ansible.com/ansible/2.8/modules/copy_module.html#copy-module" target="_blank">**copy - copy files to remote locations**  </a>** from Ansible documentation and modify the playbook file.
 
 <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/583d59bf-f616-4fbe-ad33-fb94619516e4">
</p> 
 
  - Execute the commands below to commence the exercise.

 ```
cd ansible-prjs
cp -r exercise11 exercise12
cd exercise12
vim provisioning.yaml
---

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

   - name: Banner File
     copy:
        content: '# This server is managed by Ansible, No manual changes allowed'
        dest: /etc/motd #  this is a banner file when you login to machine the content is display to user

:wq

ansible-playbook provisioning.yaml

```
- Banner file has been updated
  
 <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/567ca011-7894-4e8b-b753-847a09f48145">
</p>

- Login to any host machine to see the updated banner
  
 <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/27e03415-596c-4bf4-82a9-840167a0ffa4">
</p>
