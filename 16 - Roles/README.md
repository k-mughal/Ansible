# Roles
- In Ansible, roles are a way to organize and structure your automation content into a modular and reusable format. Roles provide a method for breaking down complex playbooks into smaller, more manageable parts. They encapsulate tasks, variables, and files, making it easier to reuse code across different projects or within the same project.
- Initially, we must pinpoint the role each server plays within our infrastructure. In our network, various computers undertake diverse responsibilities some function as MySQL servers, others as Tomcat application servers, some as Build Servers, and some as Apache servers. Additionally, there are shared configurations, such as the universal setup we implemented. Certain elements, like NTP synchronization, monitoring agents, or logging agents, are common across all servers. Some servers fulfill general roles, hosting components like NTP monitoring agents and configurations aligned with organizational standards. Meanwhile, others serve more specialized purposes, such as being dedicated MySQL or Oracle databases or functioning as application servers like Tomcat.
- 
 <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/95a026f1-e3f3-4ec5-8db5-339b6ff158dc">
</p>


Directory Structure:

1. A typical role has a predefined directory structure. The main components are:
    - **tasks:** Contains the main list of tasks that the role performs.
    - **handlers:** Contains handlers, which are special tasks that can be triggered by other tasks.
    - **defaults:** Contains default variables for the role.
    - **vars:** Contains other variable files.
    - **files:** Contains files that can be deployed as part of the role.
    - **templates:** Contains template files that can be used by tasks.
1. Main YAML File:
   - Each role has a main YAML file, usually named main.yml, within the tasks directory. This file includes the list of tasks to be executed when the role is applied.
1. Meta Information:
   - The meta directory contains a file named main.yml that includes metadata about the role, such as dependencies.
1. Handlers:
   - Handlers are special tasks that get triggered by other tasks. They are defined in the handlers directory and are often used to restart services or perform other actions in response to changes.
1. Defaults and Variables:
   - The defaults directory contains default variable values that can be overridden. Variables can also be defined in the vars directory.
- Reusability: Roles allow you to reuse automation content across different playbooks or projects, promoting code sharing and consistency.

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/d92df0fc-e141-46f1-bfa6-866f6f84047a">
</p>


- let's do an exercise to get hands on the experience about roles, first add some more tasks and variables to complicate our playbook which later came be simplified later using roles.
  
 ```
 cd ansible
 cp -r exercise13 exercise14
 cd exercise14
 vim provisioning yaml

 # add task in the task section 
  come:yes
  vars:
    mydir: /opt/dir22 # this will create a directory using copy module
tasks:
 - name: Create a folder
   file:
     path: "{{mydir}}"    
     state: directory

 - name: Dump file 
   copy:
   src: /files/myfile.txt # this file doesn't exist yet but will create it
   dest: /tmp/myfile.txt

:wq

sudo apt install tree -y # install tree package to see the folder structure in tree format 
```