- For this task, we'll employ the copy module. Please navigate to the 
  <a href="https://docs.ansible.com/ansible/2.8/modules/modules_by_category.html" target="_blank">Module Index </a>
 and select the  <a href="https://docs.ansible.com/ansible/2.8/modules/list_of_files_modules.html" target="_blank"> Files module. </a>
 - During this exercise, we'll undertake two tasks. Initially, we'll transfer the files from the control server to the host server. Subsequently, we'll add a database and create database. To achieve this, we will utilize copy module to transfer files to a remote location. So in the **File modules** click on " copy – Copy files to remote locations " and  scroll down to **Examples** then copy the first block of code.

 <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/620fa17c-2469-4b5b-9d6c-b12169c43cbb">
</p>

- Login to control server using Git Bash and run the below following commands.
  
 ```
 cd ansible-prjs
 cp -rm exercise5 exercise6
 mv web-db-playbook.yaml web-playbook.yaml (create another file)

 vim web-playbook.yaml
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

    - name: Copy index file
      copy:
        src: files/index.html
        dest: /var/www/html/index.html
        backup: yes

:wq

 ```
 - Now we need to make same file path and create a file to match with src: files/index.html, go back to Git Bash terminal and run the commands. _Note: Setting "backup: yes" will generate a backup file in case the same file already exists in the destination folder._

 ```
 mkdir files
 vim files/index.html
 Learning modules in Ansible

 :wq
 
 ansible-playbook -i inventory web-playbook.yaml
 ```
  <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/67be4ad0-7c4a-4c80-84eb-6230bc4e83eb">
</p>

- We can verify whether the index.html file has been successfully copied to the destination folder on the host server and confirm the creation of the backup file.
- Login to host server through Git Bash and follow the steps below.
  <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/221db02e-1720-4955-9c56-7b25bff4dc9b">
</p>
  

 
