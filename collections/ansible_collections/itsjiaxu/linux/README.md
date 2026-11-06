# itsjiaxu.linux

An Ansible collection for initializing Debian and Ubuntu servers with an APT
mirror, essential packages.

## Requirements

- Ansible Core 2.17 or later
- Root privileges on managed hosts (use `become: true`)

## Role: `linux_init`

The role configures an APT mirror, installs commonly used packages. Defaults are:

| Variable | Default |
| --- | --- |
| `mirror` | `https://mirrors.aliyun.com` |
| `packages` | `vim`, `git`, `curl`, `ca-certificates`, `wget`, `htop`, `net-tools` |

Example play:

```yaml
- name: Initialize Linux servers
  hosts: all
  become: true
  roles:
    - role: itsjiaxu.linux.linux_init
```

Override `mirror`, `packages`as needed. The repository and
mirror templates currently support Debian and Ubuntu.
