[![CI](https://github.com/de-it-krachten/ansible-role-package/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-package/actions?query=workflow%3ACI)


# ansible-role-package

Role to make package management easier.
Supports dnf, yum, apt, apk, zypper and pip



## Dependencies

#### Roles
None

#### Collections
- community.general

## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- CentOS 7
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- AlmaLinux 9<sup>1</sup>
- SUSE Linux Enterprise 15<sup>1</sup>
- openSUSE Leap 15
- Debian 10 (Buster)<sup>1</sup>
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Fedora 37
- Fedora 38
- Alpine 3

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
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

# max validity of apt cache
package_apt_cache_valid_time: 3600

# package repositories to disable during dnf/yum tasks
# package_disablerepo: epel

# proxy url for installing & updating packages
package_https_proxy: "{{ https_proxy | default('') }}"
package_http_proxy: "{{ http_proxy | default('') }}"
package_no_proxy: "{{ no_proxy | default('localhost,127.0.0.1') }}"
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'package'
  hosts: all
  vars:
    packages_add:
      - rsync
      - zip
    packages_rm:
      - patch
    # pip_site_upgrade: True
    python_virtualenv_root: /tmp/venv
    python_virtualenvs:
      - name: env1
        python: /usr/bin/python3
        site_packages: false
        packages:
          - pip
  roles:
    - deitkrachten.python
  tasks:

    # Skip Alpine on Ansible 2.9
    - name: Skip Alpine / Ansible 2.9
      meta: end_play
      when: ansible_distribution == 'Alpine' and ansible_version['full'] is search('^2.9')

    # dnf / yum / apt / apk

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
          - dnspython
</pre></code>
