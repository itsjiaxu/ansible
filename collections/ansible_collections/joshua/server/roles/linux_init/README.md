Linux initialization role
=========================

Installs common administration packages and sets the system timezone on Linux
hosts. Debian-family, RedHat-family, Arch Linux, and SUSE systems are supported.

Requirements
------------

The target must be a supported Linux distribution and the remote user must be
able to use `sudo`.

Role Variables
--------------

The main variables are:

* `linux_timezone`: timezone to configure, default `Asia/Shanghai`.
* `linux_mirrors`: Debian-family mirror URLs keyed by distribution name.
* `linux_configure_mirror`: whether to manage Debian-family repositories, default `false`.
* `linux_upgrade_packages`: whether to update all packages, default `false`.
* `linux_common_packages`: package lists keyed by `ansible_facts.os_family`.
* `linux_packages`: final package list installed on the target.

The role uses `apt`, `dnf`, or `yum` to refresh package metadata where
appropriate, and uses Ansible's generic `package` module for installation.
Other distributions can be added by extending `linux_common_packages` and the
supported-family assertion in `tasks/main.yml`.

Dependencies
------------


Example Playbook
----------------

Example:

    - name: Initialize Linux servers
      hosts: linux
      become: true
      roles:
        - role: joshua.server.linux_init
          vars:
            linux_timezone: Asia/Shanghai

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
