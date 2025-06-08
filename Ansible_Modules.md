# Ansible Module


An Ansible module is a reusable, standalone script that Ansible uses to perform a specific task. Modules are the core of Ansible’s functionality—they define what actions Ansible takes on target machines, such as installing packages, copying files, managing services, etc.

### 🧩 Commonly Used Ansible Modules by Category

#### 🖥️ System Modules

| Module Name | Purpose |
|-------------|---------|
| `package`   | Abstract package manager (works with apt/yum/dnf/zypper) |
| `apt`       | Manage Debian packages |
| `yum`       | Manage Red Hat-based packages |
| `dnf`       | Manage Fedora/CentOS packages |
| `service`   | Manage system services |
| `systemd`   | Interact with systemd services |
| `user`      | Manage user accounts |
| `group`     | Manage groups |
| `hostname`  | Set system hostname |
| `reboot`    | Reboot the system |
| `setup`     | Gather system facts |


#### 💻 Command Modules

| Module Name | Purpose |
|-------------|---------|
| `command`   | Run a command on remote hosts (no shell features) |
| `shell`     | Run shell commands (supports pipes, redirects, etc.) |
| `raw`       | Run raw command directly (useful for bootstrapping) |
| `script`    | Run a local script on remote hosts |
| `expect`    | Automate command-line interactions that expect input |


#### 📁 File & Directory Modules

| Module Name | Purpose |
|-------------|---------|
| `file`      | Manage file/directory attributes (permissions, ownership, etc.) |
| `copy`      | Copy files from the control node to target machines |
| `template`  | Copy and render Jinja2 templates |
| `lineinfile`| Manage lines in text files |
| `replace`   | Replace text using regex |
| `stat`      | Get file or directory status |
| `fetch`     | Download files from remote to local |
| `synchronize` | Wrapper around `rsync` |


#### 🛢️ Database Modules

| Module Name     | Purpose |
|------------------|---------|
| `mysql_db`       | Manage MySQL databases |
| `mysql_user`     | Manage MySQL users |
| `postgresql_db`  | Manage PostgreSQL databases |
| `postgresql_user`| Manage PostgreSQL users |
| `mongodb_user`   | Manage MongoDB users |
| `community.mysql.mysql_query` | Execute MySQL queries |
| `community.postgresql.postgresql_query` | Execute PostgreSQL queries |

> Note: You may need to install community collections (`community.mysql`, `community.postgresql`) via `ansible-galaxy`.


#### ☁️ Cloud Modules

| Cloud Provider | Module Collection | Purpose |
|----------------|-------------------|---------|
| AWS            | `amazon.aws`      | Manage EC2, S3, RDS, IAM, etc. |
| Azure          | `azure.azcollection` | Manage Azure VMs, storage, networking |
| GCP            | `google.cloud`    | Manage Compute Engine, Cloud SQL, etc. |
| OpenStack      | `openstack.cloud` | Manage OpenStack resources |
| VMware         | `community.vmware` | Manage vSphere, ESXi, VM lifecycle |

> Install with `ansible-galaxy collection install <collection-name>`


#### 🪟 Windows Modules

| Module Name | Purpose |
|-------------|---------|
| `win_package` | Install Windows software |
| `win_service` | Manage Windows services |
| `win_feature` | Install Windows features/roles |
| `win_copy`    | Copy files to Windows machines |
| `win_command` | Run a command (no shell features) |
| `win_shell`   | Run shell commands with PowerShell |
| `win_user`    | Manage Windows user accounts |
| `win_group`   | Manage Windows groups |
| `win_reboot`  | Reboot a Windows machine |
| `win_updates` | Manage Windows Updates |
| `win_firewall_rule` | Manage Windows Firewall rules |

> Requires WinRM configuration on the Windows host.


### 🧱 Example by Module:

Modules used in below code:-

1. Script.
2. lineinfile.
3. user.

```
- name: 'hosts'
  hosts: all
  become: yes
  tasks:
    - name: 'Execute a script'
      script: '/tmp/install_script.sh'
    - name: 'Start httpd service'
      service:
        name: 'httpd'
        state: 'started'
    - name: "Update /var/www/html/index.html"
      lineinfile:
        path: /var/www/html/index.html
        line: "Welcome to ansible-beginning course"
        create: true
    - name: "web_user"
      user:
        name: 'web_user'
        uid: 1040
        group: 'developers'                              
```

Output:
```
[bob@student-node playbooks]$ ansible-playbook -i inventory playbook.yaml 

PLAY [hosts] **********************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************
ok: [node01]
ok: [node02]

TASK [Execute a script] ***********************************************************************************************************************
changed: [node01]
changed: [node02]

TASK [Start httpd service] ********************************************************************************************************************
ok: [node01]
ok: [node02]

TASK [Update /var/www/html/index.html] ********************************************************************************************************
ok: [node01]
ok: [node02]

TASK [web_user] *******************************************************************************************************************************
changed: [node02]
changed: [node01]

PLAY RECAP ************************************************************************************************************************************
node01                     : ok=5    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ok=5    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

