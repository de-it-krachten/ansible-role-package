[![CI](https://github.com/de-it-krachten/ansible-role-package/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-package/actions?query=workflow%3ACI)


# ansible-role-package

Role to make package management easier.
Supports dnf, yum, apt and pip


Platforms
--------------

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- CentOS 7
- RockyLinux 8
- AlmaLinux 8<sup>1</sup>
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

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
    python_virtualenv_root: /tmp/venv
    python_virtualenvs:
      - name: env1
        python: /usr/bin/python3
        site_packages: false
        packages:
          - pip
  roles:
    - python
  tasks:

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
        package_pip_cmd: /tmp/venv/env1/bin/pip3
        package_list:
          - lxml
          - dnspython
</pre></code>
