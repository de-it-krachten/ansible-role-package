[![CI](https://github.com/de-it-krachten/ansible-role-package/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-package/actions?query=workflow%3ACI)


# ansible-role-package

Role to make package management easier.
Supports dnf, yum, apt and pip


Platforms
--------------

Supported platforms

- CentOS 7
- CentOS 8
- RockyLinux 8
- AlmaLinux 8
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS



Role Variables
--------------
<pre><code>
# Package manager to use (defaults to OS default)
package_mgr: "{{ ansible_pkg_mgr }}"

# Mode to operate in.
# Can be install/install-verbose/remove/remove-verbose/update/upgrade/upgrade-verbose
package_mode: install

# Should package list be updated before install/upgrade
package_update: true

# Pip command to use (can point to a virtualenv)
package_pip_cmd: /usr/bin/pip3
</pre></code>


Example Playbook
----------------

<pre><code>
- name: sample playbook for role 'package'
  hosts: all
  vars:
    packages_add:
      - rsync
      - curl
    packages_rm:
      - patch
  roles:
    - python
  tasks:

    # Setup python virtualenv

    - name: Setup venv
      pip: 
        name: wheel
        virtualenv: "{{ item }}"
        virtualenv_python: /usr/bin/python3
      loop:
        - /tmp/env1
        - /tmp/env2


    # dnf / yum / apt

    - name: 'package / os-packages / install / non-verbose'
      include_role:
        name: package
        apply:
          tags: molecule-idempotence-notest
      vars:
        package_mode: install
        package_list: "{{ packages_add }}"

    - name: 'package / os-packages / remove / non-verbose'
      include_role:
        name: package
        apply:
          tags: molecule-idempotence-notest
      vars:
        package_mode: remove
        package_list: "{{ packages_rm }}"

    - name: 'package / os-packages / install / verbose'
      include_role:
        name: package
        apply:
          tags: molecule-idempotence-notest
      vars:
        package_mode: install-verbose
        package_list: "{{ packages_rm }}"

    - name: 'package / os-packages / update / verbose'
      include_role:
        name: package
      vars:
        package_mode: upgrade-verbose


    # pip

    - name: 'package / pip / install / non-verbose / system-packages'
      include_role:
        name: package
        apply:
          tags: molecule-idempotence-notest
      vars:
        package_mode: install
        package_mgr: pip
        package_list:
          - e2j2==0.6.2

    - name: 'package / pip / update / verbose / system-packages'
      include_role:
        name: package
      vars:
        package_mode: install-verbose
        package_mgr: pip
        package_list:
          - e2j2==0.7.1

    - name: 'package / pip / install / verbose / virtualenv'
      include_role:
        name: package
      vars:
        package_mode: install-verbose
        package_mgr: pip
        package_pip_cmd: /tmp/env2/bin/pip3
        package_list:
          - ansible
          - lxml
          - dnspython
</pre></code>
