# Ansible Modules and Custom Modules

## Overview of Ansible Modules
Ansible modules are reusable, standalone scripts that perform specific tasks in Ansible playbooks. Modules enable you to automate actions such as installing packages, managing files, configuring services, or orchestrating cloud resources.

### Commonly Used Ansible Modules for Linux System Administration
1. **File Management**
   - `copy`: Copy files to remote machines.
   - `file`: Manage file attributes (e.g., permissions, ownership).
   - `lineinfile`: Ensure specific lines are present/absent in a file.
   - `find`: Search for files and directories.

2. **Package Management**
   - `apt`: Manage packages on Debian-based systems.
   - `yum`: Manage packages on Red Hat-based systems.
   - `dnf`: Modern package management for Red Hat systems.

3. **User and Group Management**
   - `user`: Create, modify, or delete users.
   - `group`: Manage groups on target systems.

4. **Service Management**
   - `service`: Manage services.
   - `systemd`: Control `systemd` services.

5. **Networking**
   - `firewalld`: Manage firewall rules.
   - `iptables`: Configure `iptables` rules.

6. **Shell and Commands**
   - `command`: Execute commands on remote nodes (non-interactive).
   - `shell`: Execute shell commands on remote nodes.
   - `script`: Run local scripts on remote machines.

7. **Others**
   - `cron`: Manage cron jobs.
   - `hostname`: Set the hostname of a machine.
   - `debug`: Print statements for debugging.

---

## Creating a Custom Ansible Module

### Why Create a Custom Module?
While Ansible provides numerous built-in modules, custom modules are useful when specific functionality is not available. Custom modules can extend Ansible's capabilities to fit unique requirements.

### Steps to Create and Test a Custom Module

#### 1. Create the Custom Module
1. Write a Python script for the module.
2. The module should include the `AnsibleModule` class from the `ansible.module_utils.basic` library.

**Example: A Custom Module to Create a Directory**
```python
#!/usr/bin/python
from ansible.module_utils.basic import AnsibleModule
import os

def main():
    module = AnsibleModule(
        argument_spec={
            'path': {'type': 'str', 'required': True},
            'state': {'type': 'str', 'choices': ['present', 'absent'], 'default': 'present'},
        }
    )

    path = module.params['path']
    state = module.params['state']

    if state == 'present':
        if not os.path.exists(path):
            os.makedirs(path)
            module.exit_json(changed=True, msg=f"Directory {path} created.")
        else:
            module.exit_json(changed=False, msg=f"Directory {path} already exists.")

    elif state == 'absent':
        if os.path.exists(path):
            os.rmdir(path)
            module.exit_json(changed=True, msg=f"Directory {path} removed.")
        else:
            module.exit_json(changed=False, msg=f"Directory {path} does not exist.")

if __name__ == '__main__':
    main()
```

#### 2. Place the Module in the Library Directory
Save the custom module in the `library/` directory of your Ansible project.

#### 3. Test the Custom Module
Create a playbook to test the module.

**Example Playbook**
```yaml
- name: Test Custom Directory Module
  hosts: localhost
  tasks:
    - name: Create a directory using the custom module
      custom_directory:
        path: "/tmp/test_directory"
        state: "present"

    - name: Remove the directory using the custom module
      custom_directory:
        path: "/tmp/test_directory"
        state: "absent"
```
Run the playbook:
```bash
ansible-playbook test_custom_module.yml
```

---

## Use Cases of Custom Modules
1. **Application-Specific Configurations**: Automating tasks unique to an organization's environment.
2. **Custom API Interactions**: Managing systems or services using APIs not supported by existing modules.
3. **Orchestration Across Multiple Systems**: Implementing workflows spanning diverse platforms.

---

## Tips for Writing Custom Modules
- Always use the `AnsibleModule` class to handle input arguments and responses.
- Use meaningful error messages for debugging.
- Follow the idempotency principle to ensure safe reruns.
- Use the `changed` parameter in `exit_json()` to indicate changes.

---

With the knowledge of built-in modules and custom modules, you can enhance automation workflows tailored to your requirements.
