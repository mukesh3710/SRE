# Ansible Introduction

## Why Ansible?
Ansible is an open-source automation tool designed for configuration management, application deployment, and orchestration. It is simple to use, agentless, and provides a clear syntax for automating tasks.

### Example:
Using Ansible, you can:
- Deploy applications to multiple servers.
- Configure web servers with specific settings.
- Orchestrate a multi-tier application setup.

## Scripts vs Ansible Playbook
Scripts often require custom coding and are specific to an environment, whereas Ansible Playbooks use YAML to define tasks and are reusable across multiple environments.

### Example:
- **Script:**
  ```bash
  #!/bin/bash
  sudo apt update && sudo apt install -y nginx
  ```
- **Playbook:**
  ```yaml
  - name: Install nginx
    hosts: webservers
    tasks:
      - name: Ensure nginx is installed
        apt:
          name: nginx
          state: present
  ```

## Use Case Example - Complex
Suppose you need to set up a complete LAMP stack (Linux, Apache, MySQL, PHP) on multiple servers:
- Scripts require a lot of manual effort and error handling.
- Ansible simplifies it with modular tasks defined in a Playbook.

---

## Ansible Configuration Files

### Default Configuration:
The default configuration file is located at `/etc/ansible/ansible.cfg`. It contains settings for:
- Inventory location.
- Privilege escalation.
- Module paths.
- Logging.

### Viewing Configuration:
- Use `ansible-config list` to view all configuration options.
- Use `ansible-config dump` to see active configurations.

### Example:
```bash
ansible-config list | grep inventory
```

---

## What is YAML?
YAML is a data serialization language used by Ansible for Playbooks and other configurations.

### Key Elements:
1. **Spaces:** Indentation is significant.
2. **Dictionary:** Key-value pairs.
3. **List:** A sequence of items.
4. **List of Dictionaries:** Complex structures.

### Examples:
- Dictionary:
  ```yaml
  app: nginx
  version: 1.18
  ```
- List:
  ```yaml
  - nginx
  - apache
  - mysql
  ```
- List of Dictionaries:
  ```yaml
  - name: nginx
    version: 1.18
  - name: apache
    version: 2.4
  ```

---

## Ansible Inventory

### Inventory Format:
Defines the hosts and groups for Ansible to manage.

### Types:
1. **INI Format:**
   ```ini
   [webservers]
   server1 ansible_host=192.168.1.1
   server2 ansible_host=192.168.1.2
   ```
2. **YAML Format:**
   ```yaml
   all:
     children:
       webservers:
         hosts:
           server1:
             ansible_host: 192.168.1.1
           server2:
             ansible_host: 192.168.1.2
   ```

### Why Different Formats?
Different formats allow flexibility and integration with dynamic inventories like cloud APIs.

### Grouping and Parent-Child Relationships:
- Group servers by roles.
- Define parent-child relationships for hierarchical management.

---

## Ansible Variables

### Usage:
Variables store data that changes with each host or environment.

### Types:
1. **String:**
   ```yaml
   app_name: nginx
   ```
2. **Number:**
   ```yaml
   max_connections: 100
   ```
3. **Boolean:**
   ```yaml
   is_active: true
   ```
4. **List:**
   ```yaml
   packages:
     - nginx
     - mysql
     - php
   ```
5. **Dictionary:**
   ```yaml
   db_config:
     user: admin
     password: secret
   ```

### Precedence:
Ansible uses a well-defined order to evaluate variables, from command-line overrides to default values.

### Register Variables:
Capture output of tasks during runtime.

```yaml
- name: Check uptime
  command: uptime
  register: result

- debug:
    msg: "Server uptime is {{ result.stdout }}"
```

### Variable Scopes:
1. **Host:** Specific to a host.
2. **Play:** Valid for a playbook run.
3. **Global:** Available across all tasks and plays.

### Magic Variables:
- **hostvars:** Access all variables of hosts.
- **groups:** Access group details.
- **group_names:** List of groups a host belongs to.
- **inventory_hostname:** Name of the current host.

### Ansible Facts:
Gather system information using `setup` module.

```yaml
- name: Gather facts
  hosts: all
  tasks:
    - name: Display OS type
      debug:
        msg: "{{ ansible_os_family }}"
```

## Ansible Playbooks

## What is a Playbook?
Ansible Playbooks are YAML files used to define a series of tasks that automate complex IT workflows. They describe what needs to be done, on which hosts, and in what order, using human-readable syntax.

### Playbook Format
- **Hosts**: Specifies the target hosts.
- **Module**: Defines the task to perform (e.g., copy, service, yum).
- **Run**: Executes the tasks sequentially.

**Example:**
```yaml
- name: Install Apache on web servers
  hosts: webservers
  tasks:
    - name: Install httpd
      yum:
        name: httpd
        state: present

    - name: Start Apache service
      service:
        name: httpd
        state: started
```

---

## Verifying Playbooks
### Why Do We Need to Verify Playbooks?
To ensure playbooks are error-free and behave as expected before running them on production systems.

### How to Verify Playbooks in Ansible
1. **Check Mode**: Tests changes without applying them.
   ```bash
   ansible-playbook playbook.yml --check
   ```

2. **Diff Mode**: Shows differences between current and intended states.
   ```bash
   ansible-playbook playbook.yml --diff
   ```

3. **Syntax Check**: Validates playbook syntax.
   ```bash
   ansible-playbook playbook.yml --syntax-check
   ```

---

## Ansible-lint
### Why Do We Need ansible-lint?
To enforce coding standards, catch errors, and improve the quality of playbooks.

### How to Use ansible-lint
Install it using pip:
```bash
pip install ansible-lint
```
Run ansible-lint on a playbook:
```bash
ansible-lint playbook.yml
```

---

## Ansible Conditionals
### Conditional - `when`
Used to execute tasks based on conditions.

**Example:**
```yaml
- name: Install httpd only on RedHat systems
  yum:
    name: httpd
    state: present
  when: ansible_facts['os_family'] == 'RedHat'
```

### Operators
- **`or`**: Used for logical OR.
- **`and`**: Used for logical AND.

### Conditionals in Loops
```yaml
- name: Install packages conditionally
  yum:
    name: "{{ item }}"
    state: present
  with_items:
    - httpd
    - nginx
  when: ansible_facts['os_family'] == 'RedHat'
```

### Conditionals & Register
```yaml
- name: Check service status
  shell: systemctl status httpd
  register: httpd_status

- name: Restart service if stopped
  service:
    name: httpd
    state: restarted
  when: httpd_status.rc != 0
```

---

## Ansible Loops
### Loops - Visualize
Use loops for repetitive tasks.
```yaml
- name: Install multiple packages
  yum:
    name: "{{ item }}"
    state: present
  loop:
    - httpd
    - mysql-server
    - php
```

### `with_*`
Ansible provides various `with_*` constructs for advanced loops.
```yaml
- name: Add users with passwords
  user:
    name: "{{ item.name }}"
    password: "{{ item.password }}"
  with_items:
    - { name: 'john', password: 'password123' }
    - { name: 'jane', password: 'password456' }
```

---

## Ansible Modules
- **`command`**: Executes a command.
- **`script`**: Runs a script on remote hosts.
- **`service`**: Manages services.
- **Idempotency**: Ensures tasks have predictable outcomes.
- **`lineinfile`**: Modifies lines in files.

**Example:**
```yaml
- name: Add a line to /etc/hosts
  lineinfile:
    path: /etc/hosts
    line: "127.0.0.1 localhost"
```

---

## Ansible Plugins
### What is a Plugin?
Plugins are modules that extend Ansible functionality.

### Key Plugins:
- **Action Plugins**: Modify tasks.
- **Callback Plugins**: Modify output.
- **Lookup Plugins**: Retrieve data.

---

## Ansible Roles
### Explain Roles
Roles organize playbooks into reusable components.

### Create Roles
```bash
ansible-galaxy init my_role
```

### Use Role
```yaml
- name: Use a role
  hosts: all
  roles:
    - my_role
```

---

## Ansible Collections
Collections are a distribution format for Ansible content.

### Benefits:
- Modular content.
- Easy sharing and reuse.

---

## Jinja2
### Templating Engine
Jinja2 is used for templating in Ansible.

### Example:
```yaml
- name: Use Jinja2 template
  template:
    src: my_template.j2
    dest: /etc/my_config.conf
```

---

## Ansible Templates
Templates allow dynamic configuration file generation.

### Example:
**Template (`my_template.j2`):**
```j2
server_name = {{ ansible_hostname }}
```

**Playbook:**
```yaml
- name: Deploy template
  template:
    src: my_template.j2
    dest: /etc/server.conf
```

---

## Ansible Install
### Install Control Node on RedHat
```bash
yum install ansible
```

### Install via PIP
```bash
pip install ansible

