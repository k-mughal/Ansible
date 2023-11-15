# Setup Ansible and Infrastructure
## Setup three EC2 Instances on AWS
  - **Step 1 :** Create an EC2 instance and refer to it as the control machine.
    - Launch instance: **Name :** control, **Image :** Ubuntu, **Instance type :** t2.micro, **Key pair :** control-keypair, **Network settings->Edit->Create security group :** control-sg, **Inbound Security Group Rules -> Source type :** My IP, **Click on :** Launch instance
- **Step2 :** Create an EC2 instance on AWS and refer to it as the host-web00
    - Launch instance: **Name :** host-web00, **Number of instances :** 3, **Image :** centOS Stream 9(x86_64), **Instance type :** t2.micro, **Key pair :** client-key, **Network settings->Edit->Create security group :** client-sg, **Inbound Security Group Rules -> Source type :** My IP, **Add security group role -> Type:Custom TCP--Port range :** 22, **Source type:Custom--Source :** control-sg, **Click on :** Launch instance
- **Step 3:** Change the host servers names
- <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/7cac2d77-8c92-4106-8c43-eb689214edbf">
</p>

## <a href="https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu" target="_blank">Install Ansible on Host Server </a>

- **Step 1 :** Copy Public IPv4 address from AWS EC2/Control Server
- <p align="center">
  <img  width="1400" height="400" src="https://github.com/k-mughal/Ansible/assets/18217530/5d5f50d2-b844-47cd-bd65-90cf953889d3">
</p>


- **Step 2 :** Launch Git Bash and establish an SSH connection to the server.
<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/beeba6e5-0e07-420d-be8e-29c97c1a625f">
</p>

- **Step 3 :** Run the following command to install Ansible

```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
ansible --version

```


