[![CI](https://github.com/de-it-krachten/ansible-role-package/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-package/actions?query=workflow%3ACI)


# ansible-role-package

Role to make package management easier.
Supports rpm (dnf/yum) and deb (apt)


Platforms
--------------

Supported platforms

- CentOS 7
- CentOS 8
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
</pre></code>


Example Playbook
----------------

<pre><code>
- name: Converge
  hosts: all
  vars:
    packages_add:
      - rsync
      - curl
    packages_rm:
      - patch
  tasks:

    - name: 'ansible-role-package / install'
      include_role:
        name: ansible-role-package
      vars:
        package_mode: install
        package_list: "{{ packages_add }}"

    - name: 'ansible-role-package / remove'
      include_role:
        name: ansible-role-package
      vars:
        package_mode: remove
        package_list: "{{ packages_rm }}"

    - name: 'ansible-role-package / update'
      include_role:
        name: ansible-role-package
      vars:
        package_mode: upgrade
</pre></code>
