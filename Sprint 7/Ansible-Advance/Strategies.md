# Ansible Strategies

Ansible strategies control how tasks are executed across a group of target hosts, influencing their order and concurrency.

**Common Strategies:**

* **`linear`:** 
    * Executes tasks sequentially on all hosts. 
    * All hosts complete the current task before moving to the next one.
    * Provides the most predictable and controlled execution but can be slower for large numbers of hosts.

* **`free`:** 
    * Executes tasks concurrently on all hosts. 
    * As soon as a host finishes a task, it starts the next one, leading to faster execution but potentially less predictable order.

* **`serial`:** 
    * Executes tasks sequentially on one host at a time. 
    * Useful when tasks have dependencies or when you need to control the order of execution precisely.
    * Can be specified with a number, e.g., `serial: 3` to execute tasks concurrently on up to 3 hosts.

* **`batch`:** 
    * Executes tasks concurrently on a specified number of hosts at a time. 
    * Useful for controlling resource usage and preventing overwhelming the target environment.

* **`round_robin`:** 
    * Executes tasks on hosts in a round-robin fashion. 
    * Can be useful for distributing load evenly across a group of hosts.

**Example:**

```yaml
- hosts: webservers
  strategy: free 
  tasks:
    - name: Install package
      apt: 
        name: httpd
        state: present
```
---

## Choosing the Right Strategy:

The choice of strategy depends on several factors, including:

- Task dependencies: If tasks have dependencies, linear or serial might be more appropriate.
- Performance requirements: free or batch can improve performance for independent tasks.
- Resource constraints: serial or batch can be used to limit the number of concurrent connections and prevent overloading target hosts.
- Control and predictability: linear provides the most control over the execution order.
