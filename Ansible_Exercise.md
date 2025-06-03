# Ansible Exercise:

## ✅ Beginner Level

### 1. Install a Package: <br>

<b> Use Case:</b> Install nginx or httpd on a remote host.<br>

<b>Tasks</b>:
- Use `apt` or `yum` module.
- Ensure the service is enabled and started.

### 2. Create a User
<b>Use Case:</b> Create a system user deploy with a home directory.

<b> Tasks: </b>
- Use the user module.
- Set shell and password (optional with vault).

### 3. Copy a File
<b> Use Case: </b> Deploy a configuration file to remote hosts.

<b>Tasks:</b>
- Use the copy module.
- Set proper permissions and owner.

### 4. Service Management

<b>Use Case:</b> Start, stop, or restart a service like nginx.

<b> Tasks:</b>
- Use the service module.
- Handle cases where service is not installed.

## 🧰 Intermediate Level

### 5. Deploy a Website

<b>Use Case:</b> Deploy static HTML files to /var/www/html.

<b>Tasks:</b>
- Install nginx.
- Use copy or template to deploy site.
- Ensure nginx is running.

### 6. Set up a Cron Job

<b>Use Case:</b> Create a cron job to back up a directory.

<b>Tasks:</b>
- Use the cron module.
- Back up /etc every day to /backup.

### 7. Configure SSH

<b>Use Case:</b> Harden SSH by updating sshd_config.

<b> Tasks:</b>

- Use the template module to push a new config.
- Restart sshd.

### 8. Install and Configure Docker

<b>Use Case:</b> Set up Docker and run a container.
<b>Tasks:</b>
- Install Docker via official repo.
- Start a simple container (e.g., hello-world or nginx).

### 💡 Bonus Advanced Exercises (Optional)
- Create reusable roles (e.g., role: nginx, role: users).
- Use Ansible Vault to encrypt sensitive data.
- Set up dynamic inventories or use tags to run partial playbooks.
- Use when, with_items, and handlers for conditional logic.
