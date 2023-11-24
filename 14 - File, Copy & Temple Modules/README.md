- In the preceding exercise, we installed NTP on the host machine,
 resulting in the creation of the NTP configuration file. Now, let's engage in some file operations by updating changes in NTP configuration file and restart the service. We can use the example from 
 **<a href="https://docs.ansible.com/ansible/2.8/modules/list_of_files_modules.html" target="_blank">**File Module**  </a>**  --> **<a href="https://docs.ansible.com/ansible/2.8/modules/copy_module.html#copy-module" target="_blank">**copy - copy files to remote locations**  </a>** from Ansible documentation and modify the playbook file.
 
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

- Access any host machine to view the refreshed banner; in this instance, we will log in to the db01-CentOS machine.
  
 <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/27e03415-596c-4bf4-82a9-840167a0ffa4">
</p>

- That was straightforward warm-up, but now we delve into the main task.We have the NTP or chrony configuration files. Our objective is to make modifications to this configuration file, and the most effective method for such updates is to create your own configuration file and then substitute it for the current one. Let's login to CentOS and one Ubuntu host machines, First copy chrony configuration file /etc/chrony.conf from Centos machine() and also copy NTP configuration file from Ubuntu host machine /etc/ntp.confg and dump it in control machine.
- Login to CentOS-db01 and follow the step below

```
# Copy chrony configuration content
ssh -i client-key.pem ec2-user@ip-172-31-32-105.ec2.internal
sudo -i
cat /etc/chrony.conf
# copy the entire content
exit
exit

# login to control machine & Paste copied content into file
ssh -i downloads/control-keypair.pem ubuntu@75.101.251.206
cd exercise12
mkdir templates
vim templates/ntpconf_centos
# paste the previously copied content into this file
:wq

# Copy NTP configuration content
# login to ubuntu host machine web03
ssh -i client-key.pem ubuntu@172-31-41-46
sudo -i
cat /etc/ntp.conf
# copy the entire content
exit
exit

# login to control machine & paste copied content into file
cd exercise12
vim templates/ntpconf_ubuntu
# past the previously copied content into this file
:wq
```

- At this point, we have duplicated the entire content from both host machines onto the control machine.
- Now we update the playbook in control machine, please note we will be using template module onwards.
  
```
vim provisioning.yaml

- name: Provisioning Servers
  hosts: all
  become: yes
  tasks:
   - name: Install ntp agent on centOS
     yum:
       name: "{{item}}" # variable
       state: present
     when: ansible_distribution == "CentOS"
     loop:
       - chrony
       - wget
       - zip
       - unzip

   - name: Install ntp agent on ubuntu
     apt:
       name: "{{item}}" # variable
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
   # Change configuration
   - name: Deploy ntp agent conf on centos
     template:
       src: templates/ntpconf_centos
       dest: /etc/chrony.conf
       backup: yes
     when: ansible_distribution == "CentOS"

   - name: Deploy ntp agent conf on ubuntu
     template:
       src: templates/ntpconf_ubuntu
       dest: /etc/ntp.conf
       backup: yes
     when: ansible_distribution == "Ubuntu"

   # Restart the servcies after configuration has been modified
   - name: Restart service on Centos
     service:
       name: chronyd
       state: restarted
       enabled: yes
     when: ansible_distribution == "CentOS"

   - name: Restart service on ubuntu
     service:
       name: ntp
       state: restarted
       enabled: yes
     when: ansible_distribution == "Ubuntu"


:wq

```

- It's worth noting why I opted for the template module over the copy module. The key distinction lies in the template module's intelligence.

- While the copy module straightforwardly takes the file and deposits it in the target location, the template module operates more intelligently. It reads the file, particularly if it involves a Jinja2 template. Now, what exactly is a Jinja2 template? It's quite an extensive concept, but in essence, it revolves around the use of variables.

- In our playbook, we define variables and subsequently utilize them. The structure involves double quotes and curly braces, constituting Jinja2 templating. With Jinja2 templating, you gain the ability to work with variables and incorporate conditions. This means you can design your configuration file based on specific conditions or even loops.

- The template module takes your template file, examines any templating you've implemented, extracts the actual content, and then transfers it to the designated target location. Let's begin with a simple example by defining a variable. we can google any ntp server, for this exercise we will use 
   **<a href="https://docs.ansible.com/ansible/2.8/modules/list_of_files_modules.html" target="_blank">**North American region.**  </a>** 

```
cd exercise12

vim group_vars/all # update the "all" file in group_vars folder

ntp0: 0. north-america.pool.ntp.org
ntp1: 1. north-america.pool.ntp.org
ntp2: 2. north-america.pool.ntp.org
ntp3: 3. north-america.pool.ntp.org

wq:


```
- With the definition of four NTP server variables, we can now incorporate them into our template.
  
```
cd exercise12
vim template/ntpconf_centos

```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/e2857fb9-736e-4030-8a7a-7ea33406151e">
</p>

```
cd exercise12
vim templates/ntpcongf_ubuntu
```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/bc2aed38-4688-47b0-885d-d493812c4494">
</p>

- If we intend to modify the NTP server details significantly,we don't have to edit it in both configuration files. just simply have to update it in the variable file, 
If that's the case, for multiple configuration files where we employ similar or identical values, we can centralize these values as variables. By adjusting the variable values as needed,we can ensure that all related configuration files are impacted.
- Now, let's run the Ansible playbook to implement these changes.

````
ansible-playbook provisioning.yaml
````
