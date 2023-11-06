# Inventory part2
- We've demonstrated the process of configuring an inventory file. Initially, we applied it to a single host. Next, we'll extend this setup to include the other two hosts.
- Additionally, we'll delve into some advanced concepts within the inventory, such as groupings and variables.
- Connect with the control server through the Git Bash terminal

- <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/15f163cb-0b1b-4827-b818-0ca53d168db2">
</p>

- Run the following commands in Git Bash terminal of the control server. _Note: ansible_host: private IP of host-web01,web02,db01 (aws)_
  
```
ls
cd ansible-prjs
ls
cp -r exercise1/ exercise2 (copy exercise1 to exercise2)
cd exercise2
ls

vim inventory (open vim editor)

all:
  hosts:
    web01:
      ansible_host: 172.31.45.71
      ansible_user: ec2-user
      ansible_ssh_private_key_file: client-key.pem
    web02:
      ansible_host: 172.31.46.137
      ansible_user: ec2-user
      ansible_ssh_private_key_file: client-key.pem
    db01:
      ansible_host: 172.31.32.105
      ansible_user: ec2-user
      ansible_ssh_private_key_file: client-key.pem

:wq
```
- Run the following commands to ssh/ping the host servers

```
ansible web01 -m ping -i inventory
ansible web02 -m ping -i inventory
ansible db01 -m ping -i inventory
```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/2feb9986-164a-4aa2-a8dd-ea83b3623c5c">
</p>

- Connecting individually to each host server is not practical, especially when dealing with a large number of hosts. Ansible offers solutions to address this challenge.
- **Grouping** in Ansible refers to the practice of organizing and categorizing hosts in an inventory file based on common attributes, roles, or purposes. This grouping simplifies the management of multiple hosts by allowing you to apply tasks, plays, and roles to entire groups of hosts, making it more efficient and organized in configuration management and automation.
  
- In the below example we will add groups in the inventory file

```
all:
  hosts:
    web01:
      ansible_host: 172.31.45.71
      ansible_user: ec2-user
      ansible_ssh_private_key_file: client-key.pem
    web02:
      ansible_host: 172.31.46.137
      ansible_user: ec2-user
      ansible_ssh_private_key_file: client-key.pem
    db01:
      ansible_host: 172.31.32.105
      ansible_user: ec2-user
      ansible_ssh_private_key_file: client-key.pem

  children:
    webservers:
      hosts:
        web01:
        web02:
    dbservers:
      hosts:
        db01:
    prod_servers:
      children:
         webservers:
         dbservers:


:wq
```
- [webservers] and [dbservers] are groups that contain specific host entries with their IP addresses.
- [prod_servers:children] is a special group that includes the child groups [web_servers] and [dbservers] This is a way to create a higher-level grouping for all the servers in a production environment.
- Now we can run the following command below to target a specific group of server servers.

```
ansible webservers -m ping -i inventory
ansible dbservers -m ping -i inventory
ansible prod_servers -m ping -i inventory

ansible prod_servers all ping -i inventory (ssh/ping all host)
ansible prod_servers '*' ping -i inventory (ssh/ping all host)

```

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/511a0040-0227-4e29-a701-713998234592">
</p>


- Earlier, we set variables at the host level, like `ansible_host`, which have the highest priority and groups have lower level priority. However, it's also possible to define variables at the group level. For instance, if we have common user and private key settings for all host servers, we can define these variables at the `prod_servers` group level.
- To further grasp the concept of variables, let's explore another example. Execute the following commands to create an additional folder by copying 'exercise2' to 'exercise3'. Run the following commands in Git Bash terminal of the control server.


```
cd ~
cd ansible-prjs/
cp -r exercise2 exercise3
cd exercise3/
ls
vim inventory

all:
  hosts:
    web01:
      ansible_host: 172.31.45.71
    web02:
      ansible_host: 172.31.46.137
    db01:
      ansible_host: 172.31.32.105

  children:
    webservers:
      hosts:
        web01:
        web02:
    dbservers:
      hosts:
        db01:
    prod_servers:
      children:
         webservers:
         dbservers:
      vars:
         ansible_user: ec2-user
         ansible_ssh_private_key_file: client-key.pem

:wq
```
- Execute the command to verify if creating a variable at the children level has been successful.
  
  ```
  ansible all -m ping -i inventory
  
  ```