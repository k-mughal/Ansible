- This section explores various Ansible configuration settings, providing an understanding of their usage and the correct order of implementation. While Ansible operates with default configurations, there may be instances where adjustments are necessary, such as changing the default SSH port from 22 to enhance security. In such cases, modifying the Ansible configuration becomes essential to align with the altered settings on servers like web or database servers.
- In Ansible, configuration settings follow a specific order of precedence, and the configuration sources are consulted in the following order:

1. **Environment Variables:** Configuration can be influenced by environment variables like `ANSIBLE_CONFIG`.
  
2. **User Configuration File:** The user-specific configuration file, usually located at `~/.ansible.cfg`, is considered.

3. **Project Configuration File:** If Ansible is executed from within a project directory, it will look for a local configuration file named `ansible.cfg` in that directory.

4. **ANSIBLE_CONFIG Environment Variable:** If set, the `ANSIBLE_CONFIG` environment variable specifies the path to the Ansible configuration file.

5. **Configuration File in the Current Working Directory:** Ansible will also check for an `ansible.cfg` file in the current working directory.

6. **System-wide Configuration File:** Lastly, Ansible will consider the system-wide configuration file, usually found at `/etc/ansible/ansible.cfg`.

- It's important to note that configurations specified later in this order take precedence over earlier ones. This hierarchy allows for flexibility in configuring Ansible at different levels, from system-wide defaults down to project-specific settings.
- More information on configuration file settings and their usage can be found in the <a href="https://docs.ansible.com/archive/ansible/2.4/intro_configuration.html" target="_blank" rel="noopener noreferrer">Ansible documentation.</a> Let's examine the default configuration file by following the steps below.

```
sudo i
vim /etc/ansible/ansible.cfg
```

  <p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/f62ada17-2a46-4a01-8081-c4564ad6ee20">
</p>

- Instead of utilizing the global configuration file, let's generate our own configuration file with the specified settings. Follow the steps below.

```
cd ansible-prjs
cd exercise7
vim ansible.cfg

[defaults]
host_key_checking = False
inventory =./inventory
forks = 5
log_path = /var/log/ansible.log

[privilege_escalation]
become = True
become_method = sudo
become_ask_pass = False


:wq

ansible-playbook db.yaml

```
- You may encounter an error "/var/log/ansible.log is not writeable" to resolve the error please follow the steps below and run the playbook db.yaml
  
```
sudo touch /var/log/ansible.log
sudo chown ubuntu.ubuntu /var/log/ansible.

```
- we can check to logs being created by using the commands below

```
cat /var/log/ansible.log
ansible-playbook db.yaml -vv

```

>[!Note]
> Ensure configuration file is always placed in the repository where the playbook is located.

