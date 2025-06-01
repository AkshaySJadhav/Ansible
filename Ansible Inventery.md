# Ansible Inventery


## 🔧 What is  Inventory?

An Ansible inventory is essentially a list of machines (hosts) that you want Ansible to manage or automate. You define this list in an inventory file, which tells Ansible:

    - What machines to connect to.
    - How to group them.
    - What connection details or variables to use

This is the starting point for any Ansible automation. The default file for inventroy would be `/etc/anisble/hosts`.

## 🧩 Inventory Structure:

This is the simple structure of the Inventery file:
```
server1.example.com
server2.example.com
server3.example.com
```

### 💡 Groups and Parent-child relationship::

You can group hosts together for easier targeting:

```
[web]
web1.server.com
web2.server.com

[db]
db1.example.com
```
Where,<br>
- `web` -> Pointing to the Web Server. <br>
- `db` -> pointing to the Database Servers.


Parent-child relationship:
```
[webserver:children]
webserver_US
webserver_UK

[webserver_US]
webserver1.us ansible_host=192.168.1.22

[webserver_UK]
webserver1.uk ansible_host=192.168.2.21
```
Where, <br>
- Group`webserver_US` and `webserver_UK` are holding the 2 hostnames.
- `Webserver` group include both `webserver_US & webserver_UK` groups.

### 🔧 Host Variables:

Set variables for a specific host:
```
web1 ansible_host=192.168.1.10 ansible_user=root ansible_user=root
db1 ansibale_host=localhost ansible
```

### 🔧 Common Inventory Parameters

These are commonly used parameters in Ansible inventory files to define how Ansible connects to and manages target hosts:

| Parameter                         | Description                                                                 |
|----------------------------------|-----------------------------------------------------------------------------|
| `ansible_host`                   | The IP address or DNS name to connect to (if different from the inventory name) |
| `ansible_user`                   | SSH username used to log in to the host                                     |
| `ansible_port`                   | SSH port number (default is `22`)                                           |
| `ansible_ssh_private_key_file`  | Path to the SSH private key for authentication                              |
| `ansible_password`               | Password for SSH authentication (used if not using key-based auth)         |
| `ansible_connection`            | Connection type (`ssh`, `local`, `docker`, `winrm`, etc.)                   |
| `ansible_become`                | Whether to use privilege escalation (`true` or `false`)                     |
| `ansible_become_user`           | The user to become after connecting (e.g., `root`)                          |
| `ansible_python_interpreter`    | Path to the Python interpreter on the target machine                        |

> ℹ️ These parameters can be set **per host**, **per group**, or **globally** in the inventory file.








