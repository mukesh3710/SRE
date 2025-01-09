## Check NFS Mount Point Status Asynchronously

# Asynchronous Execution in Ansible

Asynchronous execution allows Ansible to initiate a task on a target host and immediately proceed to the next task without waiting for the initial task to complete. This is crucial for long-running tasks that might otherwise cause timeouts or block the playbook's progress.   

async and poll:
- async: Specifies the maximum time (in seconds) allowed for the asynchronous task to complete.   
- poll: Determines the interval (in seconds) at which Ansible will check the status of the asynchronous task.
- poll: 0 instructs Ansible to start the task and immediately move on to the next task without any status checks

---

This playbook demonstrates how to check the status of an NFS mount point asynchronously in Ansible.
```yaml
---
- hosts: all
  become: true 
  tasks:
    - name: Check NFS mount point status asynchronously
      shell: "mount | grep -q '{{ nfs_mount_point }}'"
      async: 30
      poll: 5
      register: nfs_mount_check

    - name: Check NFS mount check result
      set_fact:
        nfs_mount_status: "failed" 
      when: nfs_mount_check.rc != 0

    - name: Check NFS mount check result
      set_fact:
        nfs_mount_status: "healthy"
      when: nfs_mount_check.rc == 0

    - name: Wait for NFS mount check to complete (if not already healthy)
      wait_for:
        result: success
        check_interval: 5
        timeout: 120
      when: nfs_mount_status == "failed"

    - name: Display NFS mount status
      debug:
        msg: "NFS Mount Status: {{ nfs_mount_status }}"
```


**1. Check NFS Mount Point Status**

* **`shell: "mount | grep -q '{{ nfs_mount_point }}'"`:** 
    * Executes a shell command to check if the specified `nfs_mount_point` is currently mounted.
    * `mount`: This command displays a list of currently mounted filesystems.
    * `grep -q`: This command searches for the specified `nfs_mount_point` within the output of the `mount` command.
    * `-q`: This option suppresses the output of `grep`, making the task more concise.
* **`async: 30`:** 
    * Instructs Ansible to wait for a maximum of 30 seconds for the task to start before considering it launched. 
    * This improves efficiency by allowing the playbook to proceed without immediate delays.
* **`poll: 5`:** 
    * Specifies that Ansible should check the status of the running task every 5 seconds.
* **`register: nfs_mount_check`:** 
    * Stores the results of the asynchronous task in a variable named `nfs_mount_check`.

**2. Check NFS Mount Check Result**

* **`set_fact: nfs_mount_status: "failed"`:** 
    * Sets the `nfs_mount_status` variable to "failed" if the `grep` command returns a non-zero exit code (`nfs_mount_check.rc != 0`). This indicates that the mount point was not found.
* **`set_fact: nfs_mount_status: "healthy"`:** 
    * Sets the `nfs_mount_status` variable to "healthy" if the `grep` command returns 0. This indicates that the mount point was found, and the NFS share is successfully mounted.

**3. Wait for NFS Mount Check to Complete (if not already healthy)**

* **`wait_for:`:** 
    * Pauses the playbook execution until the specified condition is met.
* **`result: success`:** 
    * Instructs the module to wait until the asynchronous task completes successfully.
* **`check_interval: 5`:** 
    * Defines the interval at which the module should check the status of the asynchronous task.
* **`timeout: 120`:** 
    * Sets a maximum waiting time of 120 seconds (2 minutes).
* **`when: nfs_mount_status == "failed"`:** 
    * Ensures that the `wait_for` module only executes if the initial mount check failed. This optimizes the playbook by avoiding unnecessary waiting when the NFS mount point is already healthy.

**4. Display NFS Mount Status**

* **`debug:`:** 
    * Used for debugging purposes.
* **`msg: "NFS Mount Status: {{ nfs_mount_status }}"`:** 
    * Displays the final `nfs_mount_status` on the console.

**Key Improvements:**

* **Asynchronous Check:** Improves performance by allowing the playbook to continue executing other tasks while the NFS mount status is being checked.
* **Immediate Result for Healthy Mounts:** Avoids unnecessary waiting when the mount point is already healthy.
* **Efficient Waiting:** The `wait_for` module is only executed when necessary.
* **Clear Status Reporting:** Provides a clear indication of the NFS mount's health.

This playbook demonstrates a robust and efficient approach to checking the status of an NFS mount point asynchronously in Ansible.
