glusterfs
=========

This role helps to install and manage glusterfs volumes.
His main purposes are :

  - Installing glusterfs.
  - Join peers to the role master.
  - Create glusterfs volumes.

For simplicity, this role is only executed on the "main node" that will delegate all needed actions to the other members.
This also means that all your brick have to share the same local path, again for simplicity and readability.

Compatibility
-------------

  - CentOS 7

Role Variables
--------------

- glusterfs_role_master

A boolean that I add for security. It only mean that the host is the "main node" of a cluster and will hold all the configurations for the other hosts.
It needs to be set to true for the role to apply.

```YAML
glusterfs_role_master: true
```

- glusterfs_cluster_members

List of all members of the cluster members, except the one you apply the role which is considered as the role master.
They need to be valid Ansible asset in the inventory for the role to apply modifications on them.

```YAML    
glusterfs_cluster_members:
  - gfs-member-2
  - gfs-member-3
```

- glusterfs_replica_volumes

List of hash containing the configuration for the replica volumes. It will be replicated on all the hosts of glusterfs_cluster_members + the role_master.
If omited cluster is defaulted to the role master and the members of the cluster.

```YAML
glusterfs_replica_volumes:
  - name: vol1
    bricks: /bricks/brick1/gv1
    cluster:
      - gfs1
      - gfs2
```

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
