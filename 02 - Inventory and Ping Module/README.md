## Inventory & Ping Module
We have a control server running the Ubuntu operating system Next, we have three machines: host-web01, host-web02, and host-DB01, all of them running CentOS 9. Our first task is to establish a connection between the control machine and its target machines (host-web01, host-web02, and host-DB01). The control machine needs to connect to these target machines, and we know that Ansible uses SSH to connect to them. To SSH into these EC2 instances, we use the command 'ssh -i [key path] [username]@[IP address]. We need to provide the same information to Ansible so that it can SSH into the target machines

>[!Note]
> Always have inventory in your repository so then you can fetch it on any machine and you can run Ansible playbooks. The default location for this file is /etc/ansible/hosts. You can specify a different inventory file at the command line using the -i  option or in the configuration using inventory. <path>


## <a href="https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html" target="_blank"> How to Build Inventory </a>
- Run the following commands in Git Bash terminal of the control server. _Note: ansible_host: private IP of host-web01 (aws)_

```
mkir ansible-prjs
cd ansible-prjs/
mkir exercise1
cd exercise1

vim inventory

all:
  hosts:
    web01:
      ansible_host: 172.31.45.71
      ansible_user: ec2-user
      ansible_ssh_private_key_file: client-key.pem

:wq
```
- Now open the Git Bash terminal and copy the client.pem key from local storage to control server
```
- Local Git Bash terminal
cat Downloads/client-key.pem
- copy the key

- Control server Git Bash terminal
vim client-key
- paste the key
- save and quit

```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/9c1a82bb-dbd2-4f33-aa8b-872833e3e31a">
</p>

- We've copied client-key.pem from the local storage folder(Downloads) and successfully inserted it into the control server. Now, it's time to proceed with executing the inventory file to shh/ping to web01.
  

```
ansible web01 -m ping -i inventory

```
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/247bbcba-299f-457d-baa2-20344ff51887">
</p>

- At this point, the terminal is interactive, requiring us to input 'yes' to proceed. 
  
- If we take a look at the error message: It indicates a failure in connecting to the host via SSH. The error message specifically mentions "Key verification failed." Since it's unable to verify the target machine without user input, we'll utilize Ansible configuration to instruct Ansible to bypass host key checks and simply proceed to accept the connection without any further confirmation.
 
-  To set it to "yes" as the default behavior. We must generate an Ansible configuration file. We can find the configuration file at /etc/ansible/ansible.cfg, although its content is quite minimal.
-  To automate the process, we will follow the steps below.
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/d1daa8c1-aa78-4d64-82ec-9791795ccc83">
</p>

```
vim ansible.cfg
```
- In vim editor enter /host_key_checking=True and press enter to search the keyword and remove the semicolon and change the value from Ture to False
  <p align="left">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/3a8fbb92-59b7-4bca-8ce6-3a6582571fff">
</p>

- First, please log out. Then, navigate to the exercise1 folder and execute the following command. You may receive an 'Permission denied' error, which can be resolved by adjusting the permissions of 'client-key.pem'.
  ```
  exit
  pwd
  ls
  ansible web01 -m ping -i inventory
  ```
  <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/a1f97429-37af-4314-8629-d6f454d1eaaa">
</p>
- Execute the command now, and it should succeed.

```
ansible web01 -m ping -i inventory
```

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/bb56982c-d22b-4a65-af41-7682f052c295">
</p>




  

