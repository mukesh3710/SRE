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
