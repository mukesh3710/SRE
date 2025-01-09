# Jinja2 Templates and Filters in Ansible

Jinja2 is a powerful templating engine that is integral to Ansible for generating dynamic content. Below are some of the most frequently used templates and filters, along with practical examples relevant to Linux system administration.

---

## Common Jinja2 Filters

### 1. `default`
- **Purpose**: Provides a default value if a variable is undefined or empty.
- **Example**:
  ```yaml
  server_name: "{{ ansible_hostname | default('localhost') }}"
  ```

### 2. `replace`
- **Purpose**: Replaces parts of a string with another string.
- **Example**:
  ```yaml
  updated_path: "{{ original_path | replace('/old', '/new') }}"
  ```

### 3. `lower` / `upper`
- **Purpose**: Converts text to lowercase or uppercase.
- **Example**:
  ```yaml
  env_var: "{{ 'Production' | upper }}"
  ```

### 4. `regex_replace`
- **Purpose**: Replaces strings using regular expressions.
- **Example**:
  ```yaml
  sanitized_user: "{{ username | regex_replace('[^a-zA-Z0-9]', '_') }}"
  ```

### 5. `join`
- **Purpose**: Joins a list of strings into a single string with a specified delimiter.
- **Example**:
  ```yaml
  path_string: "{{ paths_list | join(':') }}"
  ```

### 6. `split`
- **Purpose**: Splits a string into a list based on a delimiter.
- **Example**:
  ```yaml
  dirs: "{{ '/usr/local/bin:/usr/bin:/bin' | split(':') }}"
  ```

### 7. `map`
- **Purpose**: Applies a filter or attribute to each item in a list.
- **Example**:
  ```yaml
  file_sizes: "{{ files | map('stat.size') | list }}"
  ```

### 8. `unique`
- **Purpose**: Returns a list with duplicate elements removed.
- **Example**:
  ```yaml
  unique_items: "{{ item_list | unique }}"
  ```

### 9. `sort`
- **Purpose**: Sorts a list.
- **Example**:
  ```yaml
  sorted_items: "{{ item_list | sort }}"
  ```

### 10. `length`
- **Purpose**: Returns the number of elements in a list or characters in a string.
- **Example**:
  ```yaml
  item_count: "{{ item_list | length }}"
  ```

### 11. `to_json` / `to_yaml`
- **Purpose**: Converts data into JSON or YAML format.
- **Example**:
  ```yaml
  json_data: "{{ some_dict | to_json }}"
  ```

### 12. `basename` / `dirname`
- **Purpose**: Extracts the base name or directory name from a file path.
- **Example**:
  ```yaml
  base_name: "{{ '/etc/nginx/nginx.conf' | basename }}"
  dir_name: "{{ '/etc/nginx/nginx.conf' | dirname }}"
  ```

---

## Jinja2 Templates

### Generating Configuration Files
- **Purpose**: Customize configuration files dynamically using templates.
- **Example**: Template file `nginx.conf.j2`
  ```jinja2
  server {
      listen {{ nginx_port }};
      server_name {{ server_name }};
      root {{ document_root }};
  }
  ```
- **Usage in Playbook**:
  ```yaml
  tasks:
    - name: Configure Nginx
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
  ```

### Conditional Logic
- **Purpose**: Add logic to handle different configurations.
- **Example**:
  ```jinja2
  {% if is_ssl_enabled %}
  ssl_certificate {{ ssl_cert_path }};
  ssl_certificate_key {{ ssl_key_path }};
  {% endif %}
  ```

### Loops in Templates
- **Purpose**: Render repetitive sections dynamically.
- **Example**:
  ```jinja2
  upstream backend {
  {% for server in backend_servers %}
      server {{ server }};
  {% endfor %}
  }
  ```

---

## Real-World Scenarios for Linux Admins

### Automating `/etc/hosts` File
- **Template**: `hosts.j2`
  ```jinja2
  {% for host, ip in host_mapping.items() %}
  {{ ip }}  {{ host }}
  {% endfor %}
  ```
- **Variables**:
  ```yaml
  host_mapping:
    server1: 192.168.1.10
    server2: 192.168.1.11
  ```
- **Playbook**:
  ```yaml
  tasks:
    - name: Update hosts file
      template:
        src: hosts.j2
        dest: /etc/hosts
  ```

### Generating User Configuration
- **Template**: `user_config.j2`
  ```jinja2
  {% for user in users %}
  useradd {{ user.name }} -s {{ user.shell }} -u {{ user.uid }}
  {% if user.groups %}
  usermod -aG {{ user.groups | join(',') }} {{ user.name }}
  {% endif %}
  {% endfor %}
  ```
- **Variables**:
  ```yaml
  users:
    - name: alice
      shell: /bin/bash
      uid: 1001
      groups: ["sudo", "docker"]
    - name: bob
      shell: /bin/zsh
      uid: 1002
      groups: []
  ```

---

# Creating Custom Jinja2 Templates

Templates are files that contain static text and placeholders for dynamic content, using Jinja2 syntax. To create a custom template:
Steps:

    Create a .j2 Template File:
        Save a file with the .j2 extension, e.g., my_config.j2.

    Use Jinja2 Syntax:
        Variables: Insert placeholders using {{ variable_name }}.
        Loops: Use {% for item in list %} ... {% endfor %}.
        Conditionals: Use {% if condition %} ... {% endif %}.

Example:

### File: nginx.conf.j2
server {
    listen {{ port }};
    server_name {{ server_name }};

    location / {
        proxy_pass http://{{ backend_server }};
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

Using the Template in Ansible:

tasks:
  - name: Generate Nginx configuration
    template:
      src: templates/nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    vars:
      port: 80
      server_name: example.com
      backend_server: 127.0.0.1

# Creating Custom Jinja2 Filters

Custom filters extend Jinja2 functionality by allowing you to define your own operations.
Steps:

    Write a Python Function:
        Define a Python function to perform your custom operation.

    Register the Filter in Jinja2:
        Use the Environment class to register your filter.

    Use the Filter in Templates:
        Apply your filter using the | operator.

Example:

### File: custom_filters.py
def reverse_string(value):
    """Reverses a string."""
    return value[::-1]


## Register the filter
from jinja2 import Environment

env = Environment()
env.filters['reverse'] = reverse_string

Template Usage:

{{ "hello" | reverse }}

Output:

"olleh"

# Using Custom Filters in Ansible

Custom Jinja2 filters in Ansible can be added as part of a custom Python module.
Steps:

    Create a Filter Plugin:
        Save the custom filter in a Python file inside the filter_plugins/ directory of your role.

    Define the Filter:

### File: filter_plugins/custom_filters.py
def reverse_string(value):
    return value[::-1]

class FilterModule(object):
    def filters(self):
        return {
            'reverse': reverse_string
        }

Use the Filter in a Playbook or Role:

tasks:
  - debug:
      msg: "{{ 'hello' | reverse }}"

Run the Playbook:

    ansible-playbook your_playbook.yml

Output:

"olleh"

Best Practices

    Naming: Use descriptive names for filters to avoid conflicts.
    Testing: Test filters locally using a simple Python script.
    Documentation: Document your custom filters and templates for team members.
    Version Control: Store filters in filter_plugins/ for easy reuse and versioning.

Would you like to explore specific use cases or need additional guidance on setting up filters?

