# Error Handling in Ansible

Error handling is crucial in Ansible playbooks to ensure robustness and prevent unintended consequences

----
## ignore_errors:

- Purpose: Allows a task to continue execution even if it encounters an error.
- Use Cases: Useful when a non-zero return code from a command doesn't necessarily indicate a critical failure. For example, if a script returns a warning but the overall system state is still acceptable.
- ```yaml
  - name: Run a script that might produce warnings
  shell: /path/to/script.sh
  ignore_errors: true
  ```

## failed_when:

- Purpose: Customize the failure condition for a task. Allows you to define specific conditions that should trigger a task failure.
- Use Cases:
        Check for specific error messages in the task's output.
        Verify the existence of files or directories.
        Ensure that certain conditions are met before proceeding.
```yaml
- name: Copy a file
  copy:
    src: file.txt
    dest: /tmp/
  failed_when: "'Error' in result.stdout" 
```

## block and rescue:

- Purpose: Define a block of tasks and specify alternative actions (rescue tasks) to be executed if an error occurs within the block.
- Use Cases:
        Implement rollback mechanisms to undo changes made by previous tasks in the block if an error occurs.
        Perform alternative actions in case of unexpected failures.
```yaml
- block:
    - name: Create a directory
      file:
        path: /tmp/test_dir
        state: directory

    - name: Create a file
      file:
        path: /tmp/test_dir/test_file
        state: touch

  rescue:
    - name: Remove the directory on failure
      file:
        path: /tmp/test_dir
        state: absent
```

## handlers:

- Purpose: Define actions to be executed in response to specific events, such as task failures.
- Use Cases:
        Send notifications (e.g., email, Slack) about task failures.
        Restart services after updates or configuration changes.
```yaml
- name: Restart service on failure
  handlers:
    - name: restart_service
      service:
        name: my_service
        state: restarted

- name: Update configuration file
  copy:
    src: my_config.conf
    dest: /etc/my_service/
  notify: restart_service 
```

## retries and delay:

- Purpose: Retry a task a specified number of times with a delay between attempts.
- Use Cases:
        Handle temporary network issues or transient errors.
        Increase the likelihood of success for tasks that may occasionally fail due to external factors.
```yaml
- name: Run a command with retries
  shell: /path/to/command
  retries: 3
  delay: 5 
```

---
# Choosing the Right Error Handling Technique:

The appropriate error handling technique depends on the specific requirements of your playbook and the nature of the tasks involved. Consider the following factors:
- Severity of the error: Should the playbook stop completely, or can it continue with some tasks failing?
- Impact of the error: What are the potential consequences of the error, and how can they be mitigated?
- Frequency of the error: How often is the error likely to occur?

By carefully selecting and implementing error handling techniques, you can create more robust and reliable Ansible playbooks that can gracefully handle unexpected situations.

