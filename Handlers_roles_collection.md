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

# Ansible Roles

An Ansible role is a standardized way to organize and reuse Ansible automation content. Roles help you break down complex playbooks into smaller, reusable components, making your infrastructure code cleaner, more modular, and easier to manage.

#### 🧱 Structure of an Ansible Role

Ansible roles have a defined directory structure. Here's a basic layout:
```
my_role/
├── defaults/
│   └── main.yml           # Default variables
├── files/
│   └── example.conf       # Static files to copy
├── handlers/
│   └── main.yml           # Handlers (e.g., service restarts)
├── meta/
│   └── main.yml           # Role metadata (dependencies, etc.)
├── tasks/
│   └── main.yml           # Main list of tasks to execute
├── templates/
│   └── example.conf.j2    # Jinja2 templates
├── tests/
│   ├── inventory
│   └── test.yml           # Sample playbook for testing
├── vars/
│   └── main.yml           # Variables with higher priority than defaults
```

#### ✅ Using a Role in a Playbook


Lets create a role for a following playbook with the help of Ansible-Galaxy.

```
---
- name: Yum module demo on RHEL-based systems
  hosts: all
  become: yes
  tasks:

    - name: Install httpd (Apache web server)
      yum: name: httpd state: present

    - name: Install multiple packages
      yum:
        name:
          - git
          - wget
        state: present

    - name: Update a specific package (e.g., bash)
      yum: name: bash state: latest

    - name: Remove a package (e.g., telnet)
      yum: name: telnet state: absent

```



#### 🌐 What Is Ansible Galaxy?

Ansible Galaxy is the official community hub for sharing and downloading Ansible roles and collections. It allows you to:

    🔄 Reuse roles and collections created by others
    📦 Share your own roles
    🧩 Install roles from GitHub or Galaxy directly
    📚 Browse community-contributed automation content

Website: https://galaxy.ansible.com


#### 📁 1. Directory Structure for rhel-role

```
# ansible-galaxy init rhel-roles
- Role rhel-roles was created successfully
```

This will create the following directories:

```
rhel-role/
├── defaults/
│   └── main.yml
├── files/
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml     <-- We'll edit this
├── templates/
├── tests/
│   ├── inventory
│   └── test.yml     <-- Optional: test playbook
├── vars/
│   └── main.yml
```

#### ✍️ 2. Edit tasks/main.yml

Replace rhel-role/tasks/main.yml with the following content:

```
---
# tasks file for rhel-role

- name: Install httpd (Apache web server)
  yum:
    name: httpd
    state: present

- name: Install multiple packages
  yum:
    name:
      - git
      - wget
    state: present

- name: Update a specific package (e.g., bash)
  yum:
    name: bash
    state: latest

- name: Remove a package (e.g., telnet)
  yum:
    name: telnet
    state: absent
```


#### ▶️ 3. Use the Role in a Playbook

Create a playbook use-rhel-role.yml in the parent directory:

```
---
- name: Apply rhel-role on RHEL systems
  hosts: all
  become: yes
  roles:
    - rhel-role
```

**Run it with**: `ansible-playbook -i inventory use-rhel-role.yml`


# Ansible Collection.

#### 📦 What Is an Ansible Collection?

An Ansible collection is a packaging format that bundles multiple types of Ansible content together: <br>
✅ Roles <br>
🧰 Modules (custom plugins)<br>
🎛️ Playbooks<br>
🏷️ Plugins (lookup, filter, inventory, connection, etc.)<br>
📚 Documentation and tests <br>

Collections are the preferred way to organize and distribute content in Ansible 2.9+.

**Example:** Cisco collection can be used to gain an access to the specialized functionalities required to automate your network infrastructure effectively. Once you have installed it, you can utilize the modules and roles provided by the network.cisco collection in your playbooks and benefit from the specialized functionality it offers.


This is how you install the Ansible collection `ansible-galaxy collection install network.cisco`
