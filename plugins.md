# Ansible Pulugins & Module

In Ansible, plugins are modular components that extend Ansible’s core functionality. They allow you to customize or enhance how Ansible behaves in various parts of its execution process, like connection management, output formatting, inventory parsing, etc.

### 🔧 Common Types of Ansible Plugins

Here are the most commonly used types of plugins:

1. **Connection Plugins** <br>
    - Purpose: Define how Ansible connects to remote systems such as ssh/docker/winrm.<br>
    - Examples: ssh, paramiko, local, docker, winrm.

2. **Inventory Plugins**<br>
    - Purpose: Load inventory data from various sources like cloud providers.<br>
    - Examples: ini, yaml, aws_ec2, azure_rm, gcp_compute.<br>

3. **Callback Plugins**<br>
    - Purpose: Control how output is displayed and what happens when events occur during a playbook run.<br>
    - Examples: default, json, minimal, yaml, slack (for notifications).<br>

4. **Lookup Plugins**<br>
    - Purpose: Retrieve data from external sources databases or API inside playbooks or templates.<br>
    - Examples: file, env, passwordstore, aws_ssm.<br>

5. **Filter Plugins**<br>
    - Purpose: Transform data inside Jinja2 templates and allow you to modify the variables.<br>
    - Examples: to_nice_json, unique, regex_replace.<br>

6. **Action Plugins**<br>
    - Purpose: Customize how a module is run on the target or controller.<br>
    - Example: Used internally by modules like copy, template.<br>

7. **Vars Plugins**<br>
    - Purpose: Load variables from external sources or files.<br>
    - Examples: Load vars from HashiCorp Vault or AWS SSM.<br>

8. **Test Plugins**<br>
    - Purpose: Used in Jinja2 to return boolean results (typically with is).<br>
    - Example: is even, is defined.<br>

9. **Strategy Plugins**<br>
    - Purpose: Control the execution flow of tasks.<br>
    - Examples: linear, free, host_pinned.<br>

###  🛠️ How to Use Plugins 

Ansible comes with many built-in plugins. You can also write custom plugins or use community plugins.<br>
To enable some plugins (like inventory or callback plugins), you may need to configure ansible.cfg or place them in specific directories (e.g., ~/.ansible/plugins).<br>

Plugine: https://docs.ansible.com/ansible/latest/collections/index_module.html
