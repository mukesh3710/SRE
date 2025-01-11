# Ansible Plugins

Ansible plugins are specialized code used to augment or modify the behavior of Ansible. They help in extending Ansible's functionality in different areas such as connection methods, callbacks, lookups, and inventory management.

---

## Types of Ansible Plugins

### 1. **Action Plugins**
- Modify or extend how Ansible executes tasks.
- Example: Running tasks in a custom manner or changing their execution logic.

### 2. **Lookup Plugins**
- Fetch data from external sources or generate data dynamically.
- Example: `lookup('file', '/path/to/file')` to read content from a file.

### 3. **Filter Plugins**
- Extend Jinja2 templating functionality.
- Example: Convert a string to uppercase or filter out specific items from a list.

### 4. **Callback Plugins**
- Provide custom logging or output formatting.
- Example: Printing results in JSON format.

### 5. **Connection Plugins**
- Handle connections to hosts.
- Example: Connect to hosts via SSH, WinRM, or API endpoints.

### 6. **Inventory Plugins**
- Dynamically load inventory data from sources like databases, APIs, or files.
- Example: Fetch inventory from a CMDB or cloud service.

### 7. **Vars Plugins**
- Provide custom logic to define variables dynamically.
- Example: Pulling variables from an external API based on host information.

---

## How to Create a Custom Ansible Plugin

### Steps to Create a Filter Plugin

1. **Directory Structure:**
   ```
   custom_plugins/
   └── filter_plugins/
       └── custom_filters.py
   ```

2. **Plugin Code:** Add the following to `custom_filters.py`:
   ```python
   # custom_filters.py
   def to_uppercase(value):
       """Converts a string to uppercase."""
       if not isinstance(value, str):
           raise TypeError("Input must be a string")
       return value.upper()

   class FilterModule:
       """Custom filter plugin class."""
       def filters(self):
           return {
               'to_uppercase': to_uppercase
           }
   ```

3. **Test the Plugin:**
   ```yaml
   # test_playbook.yml
   - name: Test Custom Plugin
     hosts: localhost
     vars:
       test_string: "hello world"
     tasks:
       - name: Use custom filter to convert string to uppercase
         debug:
           msg: "{{ test_string | to_uppercase }}"
   ```
   Run the playbook:
   ```bash
   ansible-playbook -M custom_plugins test_playbook.yml
   ```

---

## How to Test Plugins

1. **Unit Testing:**
   Use Python's `unittest` module to test the functionality of plugins.
   ```python
   import unittest
   from custom_plugins.filter_plugins.custom_filters import to_uppercase

   class TestCustomFilters(unittest.TestCase):
       def test_to_uppercase(self):
           self.assertEqual(to_uppercase("hello"), "HELLO")
           with self.assertRaises(TypeError):
               to_uppercase(123)

   if __name__ == '__main__':
       unittest.main()
   ```
   Run tests:
   ```bash
   python -m unittest test_custom_filters.py
   ```

2. **Integration Testing:**
   Run the plugin in a playbook to verify integration with Ansible.

---

## Commonly Used Plugins for Linux System Administration

### 1. **Connection Plugins:**
- `ssh`: Default connection type for Linux hosts.
- `paramiko_ssh`: Alternative SSH connection using Paramiko.

### 2. **Lookup Plugins:**
- `file`: Fetch content from a file.
- `env`: Retrieve environment variables.
- `password`: Fetch passwords securely from files or APIs.

### 3. **Filter Plugins:**
- `default`: Provide default values for variables.
- `regex_search`: Extract data using regular expressions.
- `map`: Apply a filter across a list of items.

### 4. **Callback Plugins:**
- `default`: Standard Ansible output.
- `json`: Log task results in JSON format.
- `minimal`: Minimal output for CI/CD pipelines.

### 5. **Inventory Plugins:**
- `ini`: Default inventory format.
- `yaml`: Load inventory from YAML files.
- `aws_ec2`: Fetch inventory dynamically from AWS EC2.

### 6. **Vars Plugins:**
- `host_group_vars`: Use variables defined in group_vars/host_vars.
- `extra_vars`: Use variables passed via the CLI.

---

## Real-World Use Case: Callback Plugin for Logging

1. **Directory Structure:**
   ```
   custom_plugins/
   └── callback_plugins/
       └── log_results.py
   ```

2. **Plugin Code:** Add the following to `log_results.py`:
   ```python
   from ansible.plugins.callback import CallbackBase

   class CallbackModule(CallbackBase):
       CALLBACK_VERSION = 2.0
       CALLBACK_TYPE = 'notification'
       CALLBACK_NAME = 'log_results'

       def v2_runner_on_ok(self, result):
           host = result._host
           task_name = result.task_name
           self._display.display(f"Task '{task_name}' succeeded on {host.name}")
   ```

3. **Test the Plugin:**
   Add the plugin directory to your Ansible configuration:
   ```ini
   [defaults]
   callback_plugins = custom_plugins/callback_plugins
   ```
   Run any playbook and observe custom logging output.

---

## Summary
Ansible plugins are powerful tools for extending Ansible's functionality. They allow administrators to customize behavior, fetch dynamic data, and integrate with external systems. By creating custom plugins, you can tailor Ansible to suit complex workflows and specific requirements. Use plugins like connection, lookup, callback, and inventory to streamline Linux system administration tasks effectively.
