# Ansible Role-Based Deployment

This repository implements an organized and reusable structure for deploying applications using Ansible Roles. Each role corresponds to a specific component of the application, facilitating modularization and scalability. We have also transitioned from a monolithic to a distributed architecture.

---

## Repository Structure

- **`roles/`**: Contains all Ansible roles:
  - `python`: Manages Python installation and dependencies.
  - `mysql_db`: Handles MySQL database installation and configuration.
  - `flask_web`: Deploys and configures the web application.
- **`inventory`**: Defines target hosts (`db_server` and `web_server`).
- **`host_vars/`**: Contains host-specific variables:
  - `db_server.yml`
  - `web_server.yml`
- **`main_playbook.yml`**: Ties all roles together to deploy the application.

---

## Steps and Tasks

### **Role 1: `mysql_db`**
- Created using `ansible-galaxy init mysql_db`.
- Tasks moved to `roles/mysql_db/tasks/main.yml`:
  - Install MySQL database.
  - Start MySQL service.
  - Create application database.
  - Create application DB user.

### **Role 2: `flask_web`**
- Created using `ansible-galaxy init flask_web`.
- Tasks moved to `roles/flask_web/tasks/main.yml`:
  - Install Python Flask dependencies.
  - Copy web-server code.
  - Start web application.

### **Role 3: `python`**
- Created using `ansible-galaxy init python`.
- Tasks moved to `roles/python/tasks/main.yml`:
  - Install Python and its dependencies.

### **Role 4: Tying Roles Together**
- Updated `main_playbook.yml` to include roles using the `roles` directive in the following order:
  - `python`
  - `mysql_db`
  - `flask_web`
- The `tasks` directive was removed from the playbook as tasks are now handled by roles.

### **Role 5: Distributed Architecture (Database Server)**
- Updated inventory to include separate servers:
  - `db_server` for the database.
  - `web_server` for the web application.
- Moved `host_vars` into `host_vars/db_server.yml` and `host_vars/web_server.yml`.
- Modified `main_playbook.yml` to create a new play for `db_server`:
  - Targets `db_server`.
  - Applies the `python` and `mysql_db` roles.

### **Role 6: Distributed Architecture (Web Server)**
- Added a new play to `main_playbook.yml` for `web_server`:
  - Targets `web_server`.
  - Applies the `python` and `flask_web` roles.

---

## How to Use

1. **Set Up Inventory**  
   Update the inventory file with the target hosts:
   ```ini
   [db_server]
   db_host ansible_host=192.168.1.10

   [web_server]
   web_host ansible_host=192.168.1.11

---

## Role directory structure
```plain text
project/
├── inventory
├── host_vars/
│   ├── db_server.yml
│   └── web_server.yml
├── roles/
│   ├── python/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── defaults/
│   │   │   └── main.yml
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   ├── meta/
│   │   │   └── main.yml
│   │   ├── templates/
│   │   ├── files/
│   │   └── vars/
│   │       └── main.yml
│   ├── mysql_db/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── defaults/
│   │   │   └── main.yml
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   ├── meta/
│   │   │   └── main.yml
│   │   ├── templates/
│   │   ├── files/
│   │   └── vars/
│   │       └── main.yml
│   ├── flask_web/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── defaults/
│   │   │   └── main.yml
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   ├── meta/
│   │   │   └── main.yml
│   │   ├── templates/
│   │   ├── files/
│   │   └── vars/
│   │       └── main.yml
└── main_playbook.yml
```
---

## Other Files
- roles/<role_name>/defaults/main.yml : Contains default variables for the role.
