# Ansible Web Application Deployment

This repository contains an Ansible-based solution for deploying a web application with a modular and organized structure.

## Repository Structure

- **`host_vars/db_and_web_server.yml`**: Contains host-specific variables such as IP, ssh password, database name, user, and password.
- **`tasks/deploy_db.yml`**: Includes tasks for setting up and configuring the database.
- **`tasks/deploy_web.yml`**: Includes tasks for setting up and deploying the web application.
- **`main_playbook.yml`**: The main playbook that installs dependencies and includes the modular task files.

---

## Modularization Details

1. **Host Variables**  
   The database configuration (`db_name`, `db_user`, `db_password`) has been moved to `host_vars/db_and_web_server.yml` for better separation of concerns.

2. **Task Separation**  
   - `deploy_db.yml`: Manages database installation, service configuration, database creation, and user setup.  
   - `deploy_web.yml`: Handles Python Flask dependency installation, application code deployment, and application startup.

3. **Main Playbook**  
   - Focuses on installing dependencies for both database and web application setup.
   - Includes the `deploy_db.yml` and `deploy_web.yml` task files.

---

## How to Use

1. **Set Up Inventory**  
   Ensure the inventory file includes a group `db_and_web_server` pointing to the target hosts or we can put this details in host varibales file 

   Example:
   ```ini
   [db_and_web_server]
   server1 ansible_host=192.168.1.10
   ```

2. **Run the Main Playbook**
   Execute the playbook.yml to deploy the application
   ```bash
   ansible-playbook playbook.yml
   ```

---

## Key Features
   - Modular Design: Tasks are organized into reusable and manageable files.
   - Host-Specific Variables: All host-specific details are stored separately for scalability.
   - Ease of Use: Simply execute the main playbook to deploy the entire application.

---

## Notes
   - Use ansible-vault for securing sensitive variables, such as database passwords.
   - Ensure Python, Ansible, and necessary plugins are installed on the control node.

   
