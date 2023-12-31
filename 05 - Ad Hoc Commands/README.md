# Ad Hoc Commands
- **<a href="https://docs.ansible.com/ansible/latest/command_guide/intro_adhoc.html" target="_blank">**In Ansible, an ad hoc command**  </a>** is a one-time, single-task command that you can execute directly from the command line without the need to create and run a playbook. Ad hoc commands are useful for performing quick, one-off tasks or for running simple tasks on remote systems managed by Ansible.
- The basic syntax for an ad hoc command in Ansible is as follows:
  
```
ansible <hostname or group> -m <module> -a "<module arguments>"

```
- **Exercise4 :** Execute the following commands in the Git Bash terminal on the Control Server.

```
ls
cd asnsible-prjs
ls
cp - r exercise3 exercise4
ls
cd exercise4
ansible web01 -m ansible.builtin.yum -a "name=httpd state=present" -i inventory --become (note: become = sudo)

```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/ddb7e7f3-ff97-4313-a7cf-cc88109105cc">
</p>

 - Run the below following command to install packages on all servers
    
```
ansible webservers -m ansible.builtin.yum -a "name=httpd state=present" -i inventory --become 

```

- You may have observed that the current state has changed. However, if you run the same command again, it will not reinstall the package. Instead, ansible configuration system will install the package only on servers that have not previously been installed with this package.

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/bc2927f1-c4a3-4dc7-9ba8-8f37a4425470">
</p>
- To uninstall the package, you can execute the following command. This time, you will observe that the state has changed again. This is because the packages were initially installed, and the Ansible configuration system detected the change, leading to its reinstallation.

```
 ansible webservers -m ansible.builtin.yum -a "name=httpd state=absent" -i inventory --become
```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/4f849c30-68f5-44d0-bb72-0ecc7420f313">
</p>

- Let's engage in another exercise where we create an index file, push it to the host servers, and then make some changes in index file to observe how the Ansible configuration system detects these changes and takes appropriate actions.

```
vim index.html
This website is managed by Ansible

:wq

ansible webservers -m ansible.builtin.copy -a "src=index.html dest=/var/www/html/index.html" -i inventory --become

```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/7b0331df-394f-49a5-961f-430aff24ab2f">
</p>

- If we re-run this command, the output will display a "changed" status as false. However, if we make any modifications to the index file in the source or destination, the Ansible configuration system will detect the change and re-apply the change.

```
vim index.html
This website is managed by Ansible (edited)

:wq

ansible webservers -m ansible.builtin.copy -a "src=index.html dest=/var/www/html/index.html" -i inventory --become

```
- We can also test the index.html web page by pasting the IP address of the web host into your browser by making the below changes in the Security Group
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/cb963458-d93c-470c-ae04-5ec1edfc7728">
</p>

  
  



