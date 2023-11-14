- In this exercise, we will learn the process of discovering a module, utilizing it, and resolving any issues that may arise. This routine is typical for a DevOps engineer's daily responsibilities. The primary task will involve writing playbooks, each comprising various tasks. You must identify suitable modules for each task, apply them, and troubleshoot in case of any failures. This mirrors the standard workflow for a DevOps engineer.
- For this exercise, we'll employ the copy module. Please navigate to the 
  <a href="https://docs.ansible.com/ansible/2.8/modules/modules_by_category.html" target="_blank">Module Index </a>
 and select the  <a href="https://docs.ansible.com/ansible/2.8/modules/list_of_files_modules.html" target="_blank"> Files module. </a>
 - We'll undertake two tasks. Initially, we'll transfer the files from the control server to the host server. Subsequently, we'll add a database and create database users. To achieve this, we will utilize copy module to transfer files to a remote location. So in the **File modules** click on " copy – Copy files to remote locations " and  scroll down to **Examples** then copy the first block of code.

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

- In this segment, we will establish a database named "account" and include a user named "premiumUser." To accomplish this, locate the appropriate module in the documentation's  <a href="https://docs.ansible.com/ansible/2.8/modules/list_of_files_modules.html" target="_blank" rel="noopener noreferrer">Module Index.</a> Navigate to the <a href="https://docs.ansible.com/ansible/2.8/modules/list_of_files_modules.html" target="_blank" rel="noopener noreferrer"> Database modules</a>, specifically the MySQL section where you can find the module "mysql_db – Add or remove MySQL databases from a remote host". Once there, proceed to the <a href=" https://docs.ansible.com/ansible/2.8/modules/mysql_db_module.html#mysql-db-module" target="_blank" rel="noopener noreferrer"> Examples </a> section and replicate the initial code block.
  
  <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/cbae487e-16ab-4d52-8baa-f72adb854ad4">
</p>

- Login to control server using Git Bash and run the below following commands.

```
  cd ansible-prjs
  cd -r exercise5 exercise7
  mv web-db-playbook.yaml db.yaml
  vim db.yaml
---
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

    - name: Create a new database with name accounts'
        mysql_db:
        name: accounts
        state: present
  :wq

 ansible-playbook -i inventory db.yaml

```
 
- When executing the db playbook, you may encounter an error indicating missing dependencies, notably Python dependencies. Python generates Python scripts, which are deposited on the destination server (web01), and these scripts, when executed remotely, require specific Python dependencies. In this scenario, the MYSQL module is essential for Python 2.7 to establish a connection to the database, as outlined in the documentation. Many Python MySQL libraries can be installed using package managers such as YAML, APT, or PIP.

</p
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/3883cc03-e3a0-4309-a276-ed5fab1b19a2">

</p>
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/8a9b1025-53ce-481b-867d-d009b70a489c">
</p>

-  To address this issue, SSH into db01 to identify the precise package name and proceed with its installation. Initially, we'll attempt to locate the package using the grep command. If it's not found, an alternative is to install it using either through "yum install python3-PyMySQL" or via the pip package manager. However, manual installations are recommended; it is advisable to install all packages using an Ansible playbook.
  
  ```
  yum search python | grep -i mysql
  ```
  
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/6b0e3786-3679-4cb4-a6f1-ccb371176be8">
</p>
  
- Once the package name is identified, exit from the db server, login into control server and implement the following modifications below in db.yaml playbook.

```
vim db.yaml
---
- name: DBserver Setup
  hosts: dbservers
  become: yes
  tasks:
    - name: Install mariadb-server
      ansible.builtin.yum:
        name: mariadb-server
        state: present

    - name: install pymysql
      ansible.builtin.yum:
        name: python3-PyMySQL
        state: present

    - name: Start mariadb service
      ansible.builtin.service:
        name: mariadb
        state: started
        enabled: yes

    - name: Create a new database with name 'accounts'
      mysql_db:
        name: accounts
        state: present

:wq

ansible-playbook -i inventory db.yaml

```
- Now, a new error arises indicating that Ansible is unable to establish a connection. To address this issue, we can conduct an online search to explore potential solutions and troubleshoot the problem.

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/c3c24c6b-1b71-407f-8395-f76fdd81303d">
</p>
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/183b61f9-44f2-4f25-b1d5-cedb6f840a1e">
</p>

- The potential solution suggests the requirement for a socket file that facilitates the connection between two processes.

- Another troubleshooting approach is to visit the <a href="https://docs.ansible.com/ansible/latest/collections/community/mysql/index.html" target="_blank" rel="noopener noreferrer"> MySQL Ansible modules </a> community page, maintained by the Ansible community. Navigate to the "Modules" section and access the <a href="https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_db_module.html#ansible-collections-community-mysql-mysql-db-module" target="_blank" rel="noopener noreferrer">"mysql_db module."  </a> Examine the Examples and consult the Notes section for commonly encountered errors. 
  
  -  Install the module on the control server
 
 ```
  ansible-galaxy collection install community.mysql
```
 <p align="center">
<img src="https://github.com/k-mughal/Ansible/assets/18217530/aaf1bd53-0718-493d-a0e6-e697c0540630">
</p>


 
- Make adjustments to the db.yaml playbook.
  

```
---
- name: DBserver Setup
  hosts: dbservers
  become: yes
  tasks:
    - name: Install mariadb-server
      ansible.builtin.yum:
        name: mariadb-server
        state: present

    - name: install pymysql
      ansible.builtin.yum:
        name: python3-PyMySQL
        state: present

    - name: Start mariadb service
      ansible.builtin.service:
        name: mariadb
        state: started
        enabled: yes

    - name: Create a new database with name 'accounts'
      community.mysqlmysql_db:
        name: accounts
        state: present
        login_unix_socket: /var/lib/mysql/mysql.sock

```
 <p align="center">
<img src="https://github.com/k-mughal/Ansible/assets/18217530/75d67e8d-282c-4f90-ae15-ddd8b8ef0279">
</p>
- Additionally, we need to create a database user. To do this, refer to the  <a href="https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_user_module.html#ansible-collections-community-mysql-mysql-user-module" target="_blank">"mysql_user module"  </a> in the <a href="https://docs.ansible.com/ansible/latest/collections/community/mysql/index.html" target="_blank">"Modules"  </a> section. At the top of this page, it is indicates that the community.mysql module must be installed, which has already been done. Scroll down to the example section and replicate the third code block.
   <p align="center">
<img src="https://github.com/k-mughal/Ansible/assets/18217530/b22f64a3-b147-4b37-87e5-5a6323b64c3e">
</p>

- Modify the db.yaml playbook accordingly and execute it. You may encounter an "access denied" error, in this case, include the socket file path to address this issue.
  
```
vim db.yaml
---
- name: DBserver Setup
  hosts: dbservers
  become: yes
  tasks:
    - name: Install mariadb-server
      ansible.builtin.yum:
        name: mariadb-server
        state: present

    - name: install pymysql
      ansible.builtin.yum:
        name: python3-PyMySQL
        state: present

    - name: Start mariadb service
      ansible.builtin.service:
        name: mariadb
        state: started
        enabled: yes

    - name: Create a new database with name 'accounts'
      community.mysql.mysql_db:
        name: accounts
        state: present
        login_unix_socket: /var/lib/mysql/mysql.sock

    - name: Create database user with name 'premiumUser'
      community.mysql.mysql_user:
        name: premiumUser
        password: admin123
        priv: '*.*:ALL'
        state: present
        login_unix_socket: /var/lib/mysql/mysql.sock


:wq

ansible-playbook -i inventory db.yaml

```

   <p align="center">
<img src="https://github.com/k-mughal/Ansible/assets/18217530/6c2eea8a-9ba9-4e68-b5ec-7744740116cd">
</p>





