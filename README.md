# Ansible Role: ping_test

An Ansible role that tests connectivity to managed hosts using the ping module. This role supports both Linux and Windows systems, gathers operating system details, and tracks execution results for monitoring and reporting purposes.

## Features

- **Operating System Detection**: Automatically gathers and displays OS information at the start
- **Multi-Platform Support**: Handles both Linux (`ansible.builtin.ping`) and Windows (`ansible.builtin.win_ping`) hosts
- **Comprehensive Error Handling**: Uses block/rescue patterns to capture both success and failure scenarios
- **Execution Result Tracking**: Integrates with the `execution_result` role for detailed outcome reporting
- **Detailed OS Information**: Captures distribution, version, family, architecture, and hostname

## Requirements

- Ansible 2.9 or higher
- `execution_result` role must be available (for result tracking)
- Target hosts must be accessible via SSH (Linux) or WinRM (Windows)

## Role Variables

This role currently has no configurable variables in `defaults/main.yml`. All functionality is built-in.

## Dependencies

- `execution_result` role - Used to capture and report task execution outcomes

## Example Usage

### Basic Usage

```yaml
- hosts: all
  roles:
    - ping_test
```

### With Specific Host Groups

```yaml
- name: Test Linux servers
  hosts: linux_servers
  roles:
    - ping_test

- name: Test Windows servers
  hosts: windows_servers
  roles:
    - ping_test
```

### In a Playbook with Other Roles

```yaml
- name: Verify connectivity and deploy application
  hosts: web_servers
  roles:
    - ping_test
    - common
    - nginx
    - application
```

### With Tags

```yaml
- hosts: all
  roles:
    - role: ping_test
      tags: ['connectivity', 'healthcheck']
```

## What This Role Does

1. **Gathers Operating System Details**
   - Collects distribution, version, OS family
   - Displays system architecture and hostname
   - Minimal fact gathering for efficiency

2. **Performs Connectivity Test**
   - Linux: Uses `ansible.builtin.ping`
   - Windows: Uses `ansible.builtin.win_ping`
   - Registers results for downstream processing

3. **Captures Execution Results**
   - Success: Records return code and message
   - Failure: Captures stdout, stderr, exception details, and warnings
   - Integrates with `execution_result` role

## Example Output

When the role executes, you'll see output similar to:

```
TASK [ping_test : Display Operating System Information]
ok: [webserver1] => {
    "msg": [
        "Operating System: Ubuntu",
        "OS Version: 22.04",
        "OS Family: Debian",
        "System: Linux",
        "Architecture: x86_64",
        "Hostname: webserver1"
    ]
}

TASK [ping_test : Ping managed host]
ok: [webserver1]
```

## Testing

A test playbook is available in the `tests/` directory:

```bash
ansible-playbook -i tests/inventory tests/test.yml
```

## License

See the LICENSE file for license information.

## Author Information

This role is maintained by the Ansible-ServerAutomation team.

## Contributing

When contributing to this role:
- Follow Ansible best practices
- Use FQCN for all modules
- Include block/rescue for error-prone tasks
- Update this README with new features
- Test on both Linux and Windows platforms where applicable
