# ping_test role

Simple role that uses the Ansible `ping` module to verify reachability.

Usage example:

```yaml
- hosts: all
  roles:
    - ping_test
```
