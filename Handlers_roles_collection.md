# Ansible Handlers.

In Ansible, a handler is a special type of task that runs only when notified by another task. Handlers are typically used to perform actions like restarting a service, reloading configuration files, or performing cleanup—but only if something has changed during a playbook run.


### ✅ Key Characteristics of Handlers:
- They only run when notified by other tasks using notify.
- They are triggered at the end of the play, after all other tasks have finished.
- Each handler runs only once, even if it’s notified multiple times.
- You define them in the handlers: section of a playbook or role.

### 🔧 Basic Example:
```
- name: Deploy Application
  hosts: application_server

  tasks:
    - name: Copy Application Code
      Copy:
        src: app_code/
        dest: /opt/application/
      notify: Restart Application Service  # Notify the handler

  handlers:
    - name: Restart Application Service
      service:
        name: nginx
        state: restarted
```

#### 🧠 How It Works:

- Task `Copy Application Code` uses the `Copy` module to deploy the application code. It includes the notify directive.
- When this task completes, the handler restart application service with the help of `service` module. This ensures that the application service is automatically restarted on all servers whenever the application code is deployed.


### 🧾 Example: Multiple Handlers with httpd

```
- name: Configure Web Server with Apache (httpd)
  hosts: webservers
  become: yes

  tasks:
    - name: Install Apache (httpd)
      yum:
        name: httpd
        state: present

    - name: Deploy Apache config
      template:
        src: templates/httpd.conf.j2
        dest: /etc/httpd/conf/httpd.conf
      notify: Restart Apache

    - name: Allow HTTP through firewalld
      firewalld:
        port: 80/tcp
        permanent: yes
        state: enabled
        immediate: yes
      notify: Reload Firewalld

    - name: Deploy Web Application
      copy:
        src: myapp/
        dest: /var/www/html/myapp/
      notify:
        - Restart App Service
        - Restart Apache

  handlers:
    - name: Restart Apache
      service:
        name: httpd
        state: restarted

    - name: Reload Firewalld
      service:
        name: firewalld
        state: reloaded

    - name: Restart App Service
      service:
        name: myapp
        state: restarted
```
