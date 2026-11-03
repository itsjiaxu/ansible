# itsjiaxu.linux

An Ansible collection for initializing Debian and Ubuntu servers with an APT
mirror, essential packages, and a system timezone.

## Requirements

- Ansible Core 2.17 or later
- The `community.general` collection
- Root privileges on managed hosts (use `become: true`)

## Role: `linux_init`

The role configures an APT mirror, installs commonly used packages, and sets
the system timezone. Defaults are:

| Variable | Default |
| --- | --- |
| `mirror` | `https://mirrors.aliyun.com` |
| `packages` | `vim`, `git`, `curl`, `ca-certificates`, `wget`, `htop`, `net-tools` |
| `timezone` | `Asia/Shanghai` |

Example play:

```yaml
- name: Initialize Linux servers
  hosts: all
  become: true
  roles:
    - role: itsjiaxu.linux.linux_init
      vars:
        timezone: UTC
```

Override `mirror`, `packages`, or `timezone` as needed. The repository and
mirror templates currently support Debian and Ubuntu.
