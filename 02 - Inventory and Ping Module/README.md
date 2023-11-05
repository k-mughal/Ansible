## Inventory & Ping Module
We have a control machine running the Ubuntu operating system Next, we have three machines: host-web01, host-web02, and host-DB01, all of them running CentOS 9. Our first task is to establish a connection between the control machine and its target machines (host-web01, host-web02, and host-DB01). The control machine needs to connect to these target machines, and we know that Ansible uses SSH to connect to them. To SSH into these EC2 instances, we use the command 'ssh -i [key path] [username]@[IP address]. We need to provide the same information to Ansible so that it can SSH into the target machines

>[!Note]
> Always have inventory in your repository so then you can fetch it on any machine and you can run Ansible playbooks basically.The default location for this file is /etc/ansible/hosts. You can specify a different inventory file at the command line using the -i <path> option or in configuration using inventory.


## <a href="https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html" target="_blank"> How to Build Inventory </a>
- Run the following commands in Git Bash terminal of host server

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
- We've copied client-key.pem from local storage folder(Downloads) and successfully inserted into control server. Now, it's time to proceed with executing the inventory file to shh/ping to web01.

```
ansible web01 -m ping -i inventory

```
- At this point, the terminal is interactive, requiring us to input 'yes' to proceed. 
  
- Take a glance at the error message: It indicates a failure in connecting to the host via SSH. The error message specifically mentions "Key verification failed." Since it's unable to verify the target machine, we'll utilize Ansible configuration to instruct Ansible to bypass host key checks and simply proceed to accept the connection without any further confirmation.
  
  
-  To set it to "yes" as the default behavior.How can we achieve this? To implement this, we must generate an Ansible configuration file.You can find the configuration file at /etc/ansible/ansible.cfg, although its content is quite minimal.To automate the process, we will follow the steps below.
  

