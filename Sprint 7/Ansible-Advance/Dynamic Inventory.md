# Ansible Dynamic Inventory

## What is Dynamic Inventory?

Dynamic Inventory in Ansible allows you to pull host and group information from external sources rather than maintaining a static inventory file. This is particularly useful in dynamic environments, such as cloud or IT service management platforms, where the list of servers and their configurations change frequently.

### Key Features of Dynamic Inventory
- Fetch inventory dynamically at runtime.
- Support for multiple inventory plugins (e.g., AWS, Azure, ServiceNow).
- Custom scripts or plugins can be written for unsupported systems.
- Enables better scalability and flexibility.

## Why Use Dynamic Inventory?

1. **Dynamic Environments**: Ideal for cloud environments or platforms like ServiceNow CMDB.
2. **Automated Updates**: Automatically reflect changes in the inventory without manual intervention.
3. **Unified Management**: Manage inventory from a central source of truth, such as a CMDB.
4. **Efficient Grouping**: Automatically group hosts based on metadata or tags.

---

## How Dynamic Inventory Works

Dynamic Inventory uses executable scripts or plugins that:
1. Fetch inventory data from the source (e.g., ServiceNow CMDB).
2. Format the data in a specific JSON structure:
   - Hosts.
   - Groups.
   - Variables associated with hosts or groups.

---

## Real-World Example: Fetching Dynamic Inventory from ServiceNow CMDB

In this example, we'll create a Python script to fetch inventory data from ServiceNow CMDB.

### Pre-requisites
1. ServiceNow API credentials.
2. Ansible installed.
3. Python with `requests` library.

---

### ServiceNow Dynamic Inventory Script

```python
#!/usr/bin/env python3
import requests
import json
import sys

def fetch_servicenow_inventory():
    # ServiceNow API details
    url = "https://your-instance.service-now.com/api/now/table/cmdb_ci_server"
    username = "your_username"
    password = "your_password"

    headers = {
        "Content-Type": "application/json",
        "Accept": "application/json"
    }

    # Fetch data from ServiceNow
    response = requests.get(url, auth=(username, password), headers=headers)

    if response.status_code != 200:
        print(f"Error: Unable to fetch data from ServiceNow. Status Code: {response.status_code}")
        sys.exit(1)

    data = response.json()

    # Parse and organize inventory
    inventory = {"_meta": {"hostvars": {}}}
    for server in data.get("result", []):
        hostname = server.get("name")
        group = server.get("sys_class_name", "default_group")

        # Add host to group
        inventory.setdefault(group, {"hosts": []})
        inventory[group]["hosts"].append(hostname)

        # Add variables for the host
        inventory["_meta"]["hostvars"][hostname] = {
            "ip_address": server.get("ip_address"),
            "os": server.get("os")
        }

    return inventory

if __name__ == "__main__":
    if len(sys.argv) == 2 and sys.argv[1] == "--list":
        print(json.dumps(fetch_servicenow_inventory(), indent=2))
    elif len(sys.argv) == 2 and sys.argv[1] == "--host":
        # Implement host-specific details if required
        print(json.dumps({}))
    else:
        print("Usage: ./servicenow_inventory.py --list")
```

---

### How to Use the Script

1. **Save the Script**:
   Save the above script as `servicenow_inventory.py`.

2. **Make it Executable**:
   ```bash
   chmod +x servicenow_inventory.py
   ```

3. **Test the Script**:
   ```bash
   ./servicenow_inventory.py --list
   ```

   This should fetch and display the inventory in JSON format.

4. **Configure Ansible**:
   Update the `ansible.cfg` file:
   ```ini
   [defaults]
   inventory = ./servicenow_inventory.py
   ```

5. **Run a Playbook**:
   ```bash
   ansible all -m ping
   ```

---

## JSON Output Format

The script should output JSON in the following format:

```json
{
  "_meta": {
    "hostvars": {
      "host1": {
        "ip_address": "192.168.1.1",
        "os": "Linux"
      }
    }
  },
  "default_group": {
    "hosts": ["host1", "host2"]
  }
}
```

---

## Benefits of Using ServiceNow CMDB with Ansible

1. **Centralized Inventory**: ServiceNow CMDB acts as a single source of truth for all infrastructure components.
2. **Real-Time Updates**: Automatically sync inventory with any changes in the CMDB.
3. **Improved Automation**: Combine with Ansible for seamless provisioning, configuration, and orchestration.
4. **Dynamic Grouping**: Organize hosts into groups based on attributes like OS, location, or application.

---

## Best Practices

1. **Secure Credentials**: Use Ansible Vault or environment variables to secure ServiceNow API credentials.
2. **Error Handling**: Implement robust error handling for the script.
3. **Debugging**: Use `--list` and `--host` options for debugging inventory issues.
4. **Scalability**: Ensure the script can handle large datasets efficiently.

---

With this setup, Ansible can dynamically fetch inventory from ServiceNow CMDB, allowing for flexible and automated IT infrastructure management.
