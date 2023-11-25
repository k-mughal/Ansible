- In Ansible, a handler is a special kind of task that gets triggered by other tasks in a playbook. Handlers are used to perform actions like restarting a service or reloading a configuration file after certain tasks have been executed.

- Here's how handlers work in Ansible:

**1.** Definition: Handlers are defined in the playbook under the handlers section. They are named and associated with a specific task.

```
handlers:
  - name: restart apache
    service:
      name: apache
      state: restarted

```
- In this example, a handler named "restart apache" is defined to restart the Apache service.

**2**. Triggering: Handlers are not executed automatically; they are triggered by other tasks using the notify keyword.

```
tasks:
  - name: Ensure Apache is installed
    yum:
      name: httpd
      state: present
    notify: restart apache
```

- In this example, when the task to install Apache is executed, it will trigger the "restart apache" handler.

**3.** Execution: Handlers are executed at the end of a play, after all tasks have been processed. If a handler has been notified, Ansible will execute it.

- Handlers are executed only once during a play, even if they are notified multiple times. If multiple tasks notify the same handler, Ansible will still run it only once.

Here's a more complete example:

```
---
tasks:
  - name: Ensure Apache is installed
    yum:
      name: httpd
      state: present
    notify: restart apache

  - name: Ensure Apache is running
    service:
      name: apache
      state: started
    notify: reload apache config

handlers:
  - name: restart apache
    service:
      name: apache
      state: restarted

  - name: reload apache config
    command: /etc/init.d/apache2 reload

```
In this example, the first task installs Apache and notifies the "restart apache" handler. The second task ensures that Apache is running and notifies the "reload apache config" handler. The handlers are defined at the bottom of the playbook. The execution order is maintained, and handlers are run only if notified. 

- Let's engage in an exercise. In the previous exercises, a recurring issue arose where services were restarted every time the playbook was executed, instead of service should only be restarted when there was a change in the configuration file. Let's resolve this problem by employing handlers. Follow the steps below to address this issue.

```
cd ansible-prjs
cp -r exercise12 exercise13
cd exercise13
vim provisioning.yaml

#Modify the configuration
 # Change configuration
   - name: Deploy ntp agent conf on centos
     template:
       src: templates/ntpconf_centos
       dest: /etc/chrony.conf
       backup: yes
     when: ansible_distribution == "CentOS"
     notify:
       - Restart service on Centos

   - name: Deploy ntp agent conf on ubuntu
     template:
       src: templates/ntpconf_ubuntu
       dest: /etc/ntp.conf
       backup: yes
     when: ansible_distribution == "Ubuntu"
     notify:
       - Restart service on ubuntu

   # Restart the servcies after configuration has been modified
  handlers:
   - name: Restart service on Centos
     service:
       name: chronyd
       state: restarted
       enabled: yes
     when: ansible_distribution == "CentOS"


```

<p align="center">
  <img src="https://github.com/k-mughal/Ansible/assets/18217530/23b24229-8e46-4317-8df1-e5bdcfe725e7">
</p>

- For more information visit
**<a href="https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_handlers.html" target="_blank">**Ansible Documentation**  </a>** for handlers.
