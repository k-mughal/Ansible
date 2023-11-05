# Setup Ansible and Infrastructure
## Create and run a playbook with Ansible
  - **Step 1 :** Set up an EC2 instance on AWS and refer to it as the control machine.
    - Launch instance: **Name**: control, **Image**: Ubuntu, **Instance type**: t2.micro, **Key pair**: control-keypair, **Network settings->Edit->Create security group**: control-sg, **Inbound Security Group Rules->Source type**: My IP, **Click on**: Launch instance
- **Step2 :** Create an EC2 instance on AWS and refer to it as the host-web00
    - Launch instance: **Name**: host-web00, **Number of instances**: 3, **Image**: centOS Stream 9(x86_64), **Instance type**: t2.micro, **Key pair**: client-key, ***Network settings->Edit->Create security group**: client-sg, **Inbound Security Group Rules->Source type**: My IP, **Add security group role->Type:Custom TCP--Port range** 22: **Source type:Custom--Source**: control-sg** , Click on: Launch instance
- **Step 3:** Change the host servers names
- <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/7cac2d77-8c92-4106-8c43-eb689214edbf">
</p>
  


