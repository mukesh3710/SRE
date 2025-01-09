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

### 1. Generating Configuration Files
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

### 2. Conditional Logic
- **Purpose**: Add logic to handle different configurations.
- **Example**:
  ```jinja2
  {% if is_ssl_enabled %}
  ssl_certificate {{ ssl_cert_path }};
  ssl_certificate_key {{ ssl_key_path }};
  {% endif %}
  ```

### 3. Loops in Templates
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

### 1. Automating `/etc/hosts` File
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

### 2. Generating User Configuration
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

