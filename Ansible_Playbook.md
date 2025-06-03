# Ansible Playbook

An Ansible playbook is a YAML file that defines a series of automation steps (called tasks) to be executed on a group of hosts. It's the core way Ansible performs configuration management, software installation, orchestration, and more.

Here's a simple Ansible setup to install httpd (Apache web server) on server1 and server2 using yum.

### ✅ Step 1: Create the Inventory File

Save this as inventory.ini:
```
[webservers]
server1
server2
```
- Replace server1 and server2 with the actual hostnames or IP addresses.
- Make sure Ansible can SSH into them (SSH key or password-based).

### ✅ Step 2: Create the Ansible Playbook

Save this as install_httpd.yml:
```
---
- name: Install httpd on webservers
  hosts: webservers
  become: true   # Use sudo
  tasks:
    - name: Install httpd using yum
      yum:
        name: httpd
        state: present
```
### ✅ Step 3: Run the Playbook

Use this command:
```
ansible-playbook -i inventory.ini install_httpd.yml
```

### 🔧 Optional: Testing Connectivity

Before running the playbook, test the connection:
```
ansible -i inventory.ini webservers -m ping
```
- If it responds with "pong", you're good to go.


### 🧩 Ansible sample playbook with 2 play and multiple tasks:

The tasks will:

- Install execute the date command and run `test_python.py` on server1
- Ensure the httpd service is installed and started on server2.

```
---
- name: Play-1
  hosts: server1
  tasks:
    - name: Execute command date
      command: date

    - name: Execute script.
      script: test_python.py

- name: Play-2
  hosts: server2
  tasks:
    - name: Install httpd
      yum:
          name: httpd
          state: present

    - name: Start and enable httpd service
      service:
          name: httpd
          state: started
```

There are 2 plays in above playbook and each of them have two tasks. 

- Play are organised as a Directory in the JSON format which is unorder list, Therefore you can make changes in sequance. 
- The Tasks are written as a List, hence we can not change the sequance of the task.
- `Yum` , 'service' , 'script' are the modules in the above playbooks.


### Verifying the Playbooks:

- Verifying Ansible playbooks helps prevent configuration errors and ensures they execute as intended.
- It reduces the risk of downtime and service disruptions by catching issues before deployment.
- Verification ensures idempotency and security, avoiding unnecessary changes and unsafe practices.
- It supports compliance, improves team collaboration, and saves time by identifying problems early.

These are modes Ansible offers:

1. <b>Normal Mode</b>: This is the default mode where Ansible executes tasks and applies actual changes to the target systems.

2. <b>Check Mode (--check)</b>: Also known as "dry run" mode, it simulates the execution of the playbook without making any changes, allowing you to preview what would happen.

3. <b>Diff Mode (--diff)</b>: Shows the differences between the current state and the proposed changes, useful especially for files and templates when used with --check or normal mode.

4. <b>Syntax Check Mode (--syntax-check)</b>: This will verify the syntax error from the Playbook file.

5. <b>Verbose Mode</b> (-v, -vv, -vvv, -vvvv): Provides detailed output about what Ansible is doing, helpful for debugging and understanding task execution.

### Ansible-lint tool:

ansible-lint is a command-line tool used to check Ansible playbooks, roles, and tasks for best practices and potential issues. 

 ```
# ansible-lint playbook.yml 
  ```
This command checks playbook.yml for problems and lists them with helpful explanations.


### Ansible Conditions

In Ansible, conditions allow you to control when a task, block, or role runs, based on variables, facts, or expressions. This is typically done using the `when` keyword.

🔹 Basic `when` condition
```
- name: Install nginx only on Ubuntu
  apt:
    name: nginx
    state: present
  when: ansible_facts['os_family'] == "Debian"
```

🔹 Multiple Conditions (AND logic)

```
- name: Install only if OS is Debian and memory is more than 1GB
  debug:
    msg: "Meets all conditions"
  when:
    - ansible_facts['os_family'] == "Debian"
    - ansible_facts['memtotal_mb'] > 1024
```

🔹 OR Conditions
```
- name: Run if user is admin or dev
  debug:
    msg: "User is admin or dev"
  when: user == 'admin' or user == 'dev'
```

## Ansible loops:

In Ansible, loops let you repeat a task multiple times with different items. The most common looping method is using the loop keyword, but there are others like with_items, with_dict, with_nested, etc.
```
- name: Install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - nginx
    - git
    - curl
```
