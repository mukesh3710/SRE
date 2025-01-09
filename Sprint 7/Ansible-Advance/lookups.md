# Ansible Lookups with Examples

In Ansible, **lookups** allow you to access data from external sources and use it within your playbooks. Lookups are evaluated on the controller (local machine) rather than the target hosts, making them useful for fetching data from files, environment variables, or external APIs.

---

## **Common Lookups and Examples**

### **1. File Lookup**
Fetch the content of a file from the local filesystem.

#### **Example:**
```yaml
tasks:
  - name: Read content of a file
    debug:
      msg: "{{ lookup('file', '/path/to/local_file.txt') }}"
```

#### **Output:**
If `/path/to/local_file.txt` contains `Hello, World!`:
```
TASK [Read content of a file] ************************************************
ok: [localhost] => {
    "msg": "Hello, World!"
}
```

---

### **2. Password Lookup**
Generate or fetch a password stored in a file or other vault systems.

#### **Example:**
```yaml
tasks:
  - name: Generate or retrieve a password
    debug:
      msg: "{{ lookup('password', '/dev/null length=10') }}"
```

#### **Output:**
```
TASK [Generate or retrieve a password] ***************************************
ok: [localhost] => {
    "msg": "A random 10-character password, e.g., '3fj1#V9xTy'"
}
```

---

### **3. Env Lookup**
Fetch the value of an environment variable.

#### **Example:**
```yaml
tasks:
  - name: Get environment variable
    debug:
      msg: "{{ lookup('env', 'HOME') }}"
```

#### **Output:**
```
TASK [Get environment variable] **********************************************
ok: [localhost] => {
    "msg": "/home/username"
}
```

---

### **4. Items Lookup**
Fetch items from a list or dictionary.

#### **Example:**
```yaml
tasks:
  - name: Iterate over a list of items
    debug:
      msg: "{{ item }}"
    with_items: "{{ lookup('items', 'apple,banana,orange', split=',') }}"
```

#### **Output:**
```
TASK [Iterate over a list of items] ******************************************
ok: [localhost] => (item=apple) => {
    "msg": "apple"
}
ok: [localhost] => (item=banana) => {
    "msg": "banana"
}
ok: [localhost] => (item=orange) => {
    "msg": "orange"
}
```

---

### **5. Query Lookup (Similar to Variables)**
Fetch values from a variable or host/group.

#### **Example:**
```yaml
vars:
  my_list:
    - item1
    - item2

tasks:
  - name: Query variable
    debug:
      msg: "{{ lookup('query', 'my_list') }}"
```

#### **Output:**
```
TASK [Query variable] ********************************************************
ok: [localhost] => {
    "msg": [
        "item1",
        "item2"
    ]
}
```

---

### **6. DNS Lookup**
Fetch DNS records.

#### **Example:**
```yaml
tasks:
  - name: Perform DNS lookup
    debug:
      msg: "{{ lookup('dig', 'example.com') }}"
```

#### **Output:**
```
TASK [Perform DNS lookup] ****************************************************
ok: [localhost] => {
    "msg": "IP address of example.com"
}
```

---

### **7. Template Lookup**
Render a Jinja2 template and fetch its content.

#### **Example:**
```yaml
tasks:
  - name: Render a Jinja2 template
    debug:
      msg: "{{ lookup('template', 'templates/my_template.j2') }}"
```

#### **Output:**
The template content is rendered and displayed.

---

### **8. CSV Lookup**
Read data from a CSV file.

#### **Example:**
```yaml
tasks:
  - name: Read from CSV
    debug:
      msg: "{{ lookup('csvfile', 'path/to/file.csv col=1 skip=1') }}"
```

---

## **Syntax for Lookups**
```yaml
{{ lookup('<plugin_name>', '<parameters>') }}
```

## **Usage Scenarios**
- Fetching secrets from a vault.
- Dynamically generating configurations from templates.
- Accessing external data sources like databases, DNS, or APIs.
- Handling local files for dynamic playbook behavior.
