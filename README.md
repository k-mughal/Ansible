# ANSIBLE INTRODUCTION
## History of Automatinon
### Perspective
- Bash Scripting / Batch Scripting
- Paython / Perl / Ruby
- Puppet
- Ansible

## Use Cases
- Automation (Any System Automation)
- Change Management (Producton Server changes)
- Provisioning (Setup Servers from scratch/ Cloud provisioning)
- Orchestration (Large scale automation framwork)
  
## Asnible is Smiple
- No Agents
  - Target machines/Services are accessed by SSH(linux), winrm(windows) & API
- No Database
  - YAML, INI & Texts
- No Complex Setup
  - Its just a Python Library
- No Residual Software
  - Push Python package
  - Execute
  - Return Output
## YAML
  - No Programming
  - Structured
  - Easy to Read and write
## API
  - URL/Restful Calls(e.g Cloud - Boto(aws))
  - Shell Commands (e.g windows powershell commands)
  - Scripts
## Ansible Connections
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/8ac061a5-9f19-409b-84ea-aa7254fefc43">
</p>

- **SSH :** Linux Server operating system, Ubuntu Server, CentOS, RHEL will have SSH running. So we don't need to set up anything, just need SSH detail and Ansible can connect to Linux machine using SSH.
- **WINRM :** This service already in windows. In Windows PowerShell, we have to enable the remote connection so we can connect to Windows through Winrm connection from remotely.

- **API :**  For cloud like AWS, Azure, Google Cloud, or any other cloud, it uses APIs. You can also manage network devices like switches and routers through Ansible or even databases. For database, it's going to use its own Python libraries. Like for mysql it has Python MySQL Library. So we don't need to set up an agent on the destination or in the ansible control machine.
  

## Ansible Architecture
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/55f0b3f2-268b-4e6f-a21d-5a24182d9c34">
</p>

- **Inventory :** It refers to a list of target hosts that Ansible will manage or interact with. It has target machine information, for example IP address, usernames password will be mentioned in the inventory file.
    - Below is an example of an inventory file in YAML format:
 
```
all:  # This is a group that includes all hosts

  web_servers:  # This is a group named web_servers
    hosts:
      web1.example.com:
        ansible_host: 192.168.1.101
        ansible_port: 22
        ansible_user: your_username
        ansible_ssh_pass: your_password
      web2.example.com:
        ansible_host: 192.168.1.102
        ansible_port: 22
        ansible_user: your_username
        ansible_ssh_key: /path/to/your/ssh/key

  db_servers:  # This is a group named db_servers
    hosts:
      db1.example.com:
        ansible_host: 192.168.1.201
        ansible_port: 22
        ansible_user: your_username
        ansible_ssh_key: /path/to/your/ssh/key
```
  - **Playbooks :** It is a configuration and automation script written in YAML that defines a set of tasks and configurations to be applied to one or more hosts. Playbooks are at the core of Ansible automation and provide a way to describe the desired state of a system.

``` 
- name: Example Playbook
  hosts: web_servers  # This specifies the group of hosts from the inventory

  tasks:
    - name: Install Apache package
      become: yes  # This task requires privilege escalation (sudo)
      apt:
        name: apache2
        state: present
      when: ansible_os_family == "Debian"  # Conditional task for Debian-based systems

    - name: Install Nginx package
      become: yes
      yum:
        name: nginx
        state: present
      when: ansible_os_family == "RedHat"  # Conditional task for Red Hat-based systems

    - name: Create a sample HTML file
      become: yes
      file:
        path: /var/www/html/index.html
        state: touch
      notify: Restart Apache Service

  handlers:
    - name: Restart Apache Service
      become: yes
      service:
        name: apache2
        state: restarted

```
- ##### The playbook is named "Example Playbook.
- ##### It targets hosts from the "web_servers" group in the inventory.
- ##### It includes three tasks:
    - Installing the Apache package on Debian-based systems (e.g., Ubuntu).
    - Installing the Nginx package on Red Hat-based systems (e.g., CentOS).
    - Creating a sample HTML file on the remote hosts.
- ##### Privilege escalation (become: yes) is required for tasks that need administrator permissions, like package installation.
- ##### Conditional statements (when) are used to execute tasks only on specific host types (Debian or Red Hat).
- ##### A handler named "Restart Apache Service" is defined, which can be triggered by tasks using the notify keyword. This handler restarts the Apache service when needed.
- ##### To run this playbook, you can use the ansible-playbook command:
```
ansible-playbook -i inventory.yml your_playbook.yml
```

- **Modules :** It is a standalone, reusable script or command that performs a specific task on remote hosts. Modules are the building blocks for tasks in Ansible playbooks and are responsible for carrying out actions like installing packages, managing files, starting services, and more. Ansible includes a wide range of built-in modules for common automation tasks.

Here's an example of using the apt module to install a package on a Debian-based system (e.g., Ubuntu):
```
---
- name: Install Apache on Debian
  hosts: web_servers
  become: yes  # This task requires privilege escalation (sudo)

  tasks:
    - name: Install Apache package
      apt:
        name: apache2
        state: present

```
- #### In this example:
    - The name field specifies the name of the task.
    - The task is run on hosts from the "web_servers" group.
    - Privilege escalation (become: yes) is used because installing packages typically requires administrator permissions.
    - The apt module is used to install the "apache2" package. The state parameter is set to "present," indicating that the package should be installed if it is not already.
 
      
 
 <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/db43d158-01e4-4b50-8e2d-162e6843dd51">
</p>




