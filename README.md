[![CI](https://github.com/de-it-krachten/ansible-role-package/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-package/actions?query=workflow%3ACI)


# ansible-role-package

Role to make package management easier.
Supports rpm (dnf/yum), deb (apt) and pip.



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
# Can be install/remove/update/upgrade
package_mode: install
</pre></code>


Example Playbook
----------------

<pre><code>
- name: Converge
  hosts: all
  vars:
  tasks:
    - name: "Include role 'ansible-role-package'"
      include_role:
        name: "ansible-role-package"
</pre></code>
