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



