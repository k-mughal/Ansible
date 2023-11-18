- In the previous exercise, we learned how to define and utilize variables within a playbook and how to print them. Now, in this exercise, We'll focus on inventory-based variables for groups and hosts, along with common variables, and understand their priority. Additionally, we will define variables outside the playbook for improved reusability. Login to host server through Git Bash and follow the steps outlined below.
   
```
~/ansible-prjs/exercise8$ mkdir group_vars
~/ansible-prjs/exercise8$ vim group_vars/all

dbname: sky
dbuser: pilot
dbpass: aircraft

:wq

```
- We've defined identical variables in the folder(group_vars/all). When you execute the playbook, you'll observe that variables defined in the playbook have taken precedence, the above defined variables(group vars) unused. Refer to the playbook output below for confirmation.

```
ansible-playbook db.yaml

```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/5250360d-1545-4783-94bb-0c6a036fc523">
</p>

- Next, we can comment out the variables specified in the playbook and rerun it. It will initially search for variables within the playbook. Since we have removed them, the playbook will then search for variables in group_vars/all based on its structure. Please refer to the following steps.

```
vim ansible-playbook db.yaml

```

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/8a7ce967-6475-41d0-9bb1-bebef06fb415">
</p>

```
ansible-playbook db.yaml
```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/b5fe4024-ca35-44c3-9fd8-6a20e1cd3b65">
</p>

- Variables can be defined in various locations. Follow the steps below to create an additional exercise, In a simple task we will create a user and assign specific variable to that user. During the execution, the system will create the user and seek user variable to use them. We need to employ the 'register' module to capture the output and use the 'debug' module to view the results.

```
cd ansible-prjs
cp -r exercise8 exercise9
cd exercise9
rm -rf db.yaml/
rm -rf groups_var

vim vars-precedence-playbook.yaml
- name: Understanding vars
  hosts: all
  become: yes
  vars:
    USRNM: playuser
    COMM: varialbe from playbook
  tasks:
    - name: create user
      ansible.builtin.user:
        name: "{{USRNM}}"
        comment: "{{COMM}}"
      register: usrout

    - debug:
        var: usrout.name

    - debug:
        var: usrout.comment


:wq

ansible-playbook vars_precedence-playbook.yaml
```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/a3834147-9c80-4ece-80a7-806542c87934">
</p>

- Let's set variables at the group level, repeating the same task we accomplished in exercise 8. 

```
ansible-prjs/
ansible-prjs$ cd exercise9
ansible-prjs/exercise9$ mkdir group_vars
ansible-prjs/exercise9$ vim group_vars/all

USRNM: commonuser
COMM: variable from groupvars_all file

:wq

```

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/dc32c4d4-4be9-42c9-8bcc-27453cb28426">
</p>


- Execute the playbook
  
```
ansible-playbook vars-precedence-playbook.yaml
```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/ddd6d3be-fd74-4280-8a13-c157719afa9d">
</p>



- Lets create a dedicated file for variable 'webservers' (group_vars/webservers). Consequently, 'web01' and 'web02' will utilize variables from the 'webservers' file, while 'db01' will utilize variables from the 'group_vars/all' file as no separate file defined for db01. Note: ensure vars section in the vars_precedence.yaml is commented out otherwise these variable will take high priority.

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/82623b1c-301f-4352-a6ec-ad8f363e2e2f">
</p>

- Now, we'll proceed to define group-level variables
  
```
cd exercise9
vim group_vars/webservers
USRNM: webgroup
COMM: variable from group_vars/webservers file

:wq

ansible-playbook vars-precedence-playbook.yaml
```

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/cb63ddcc-983d-4a92-a48d-2542fd901947">
</p>

- we can create variables at host level also, please follow the steps below

```
cd exercise9
mkdir host_vars
vim host_vars/web02
USRNM: web02
COMM: varialbes from host_vars/web02 file

:wq

```
- The snippet below illustrates that all variables are identical, but their values differ.

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/4434b1b1-219e-4f20-a679-ee564271b5b1">
</p>

- Lets execute the playbook

```
ansible-playbook vars-precedence-playbook.yaml
```

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/853e16b4-e4bc-407c-af89-4476e8ba6f8b">
</p>


- Another method of passing variables through the command line is using the "-e" option, command-line-passed variables have the highest priority and can override playbook-defined variables. first uncomment the vars in playbook and run the playbook.


<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/350c14e5-e4c4-435a-a258-b98fead97d43">
</p>



```
ansible-playbook -e USRNM=cliuser -e COMM=cliuser vars-precedence-playbook.yaml
```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/f1a52a36-1a91-4d5f-8a58-e027b0bd1c03">
</p>

>[!Note] It is very uncommon to define variables directly inside the playbook. Typically, the 'all' file is utilized within the group_vars folder, and additional files are employed based on specific requirements, such as host or group-level variable files.
- Please refer to the Ansible documentation for use of 
 **<a href="https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html" target="_blank">**simple variables**  </a>**