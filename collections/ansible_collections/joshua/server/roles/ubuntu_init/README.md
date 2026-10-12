Ubuntu initialization role
=========================

Configures Ubuntu package repositories, installs common administration packages,
and sets the system timezone.

Requirements
------------

The target must be Ubuntu and the remote user must be able to use `sudo`.

Role Variables
--------------

The main variables are:

* `ubuntu_timezone`: timezone to configure, default `Asia/Shanghai`.
* `ubuntu_mirror`: Ubuntu mirror base URL.
* `ubuntu_configure_mirror`: whether to manage Ubuntu repositories, default `true`.
* `ubuntu_upgrade_packages`: whether to run `dist` upgrade, default `false`.
* `common_packages`: packages installed on every target.

Dependencies
------------


Example Playbook
----------------

Example:

    - name: Initialize Ubuntu servers
      hosts: ubuntu
      become: true
      roles:
        - role: joshua.server.ubuntu_init
          vars:
            ubuntu_timezone: Asia/Shanghai

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
