#Ad Hoc Commands
- **<a href="https://docs.ansible.com/ansible/latest/command_guide/intro_adhoc.htm" target="_blank">**In Ansible, an ad hoc command**  </a>** is a one-time, single-task command that you can execute directly from the command line without the need to create and run a playbook. Ad hoc commands are useful for performing quick, one-off tasks or for running simple tasks on remote systems managed by Ansible.
- The basic syntax for an ad hoc command in Ansible is as follows:
  
```
ansible <hostname or group> -m <module> -a "<module arguments>"

```
** Exercise4 : ** execute the following commands in the Git Bash terminal on the Control Server.

```
ls
cd asnsible-prjs
ls
cp - r exercise3 exercise4
ls
cd exercise4
ansible web01 -m ansible.builtin.yum -a "name=httpd state=present" -i inventory --become (note: become = sudo)

```
- PICTURE
  
- You may have observed that the current state has changed. However, if you run the same command again, it will not reinstall the package. Instead, ansible configuration system will install the package only on servers that have not previously been installed with this package.
  
```
ansible webservers -m ansible.builtin.yum -a "name=httpd state=present" -i inventory --become 

```
  
- 

