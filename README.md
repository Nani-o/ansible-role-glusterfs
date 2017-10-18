glusterfs
==========

This role helps to install and manage glusterfs volumes.
His main purposes are :

  - Installing glusterfs.
  - Join peers to the master.
  - Create glusterfs volumes.

For simplicity, this role is only executed on the "main node" that will delegate all needed actions to the other members.
This also means that all your brick have to share the same local path, again for simplicity and readability.

Compatibility
-------------

  - CentOS 7

Role Variables
--------------

Variables

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: ansible-role-glusterfs }

License
-------

MIT

Author Information
------------------

Sofiane MEDJKOUNE
