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

- glusterfs_distributed_volumes

List of hash containing the configuration for the distributed volumes. It will be replicated on all the hosts of glusterfs_cluster_members + the role_master.
If omited cluster is defaulted to the role master and the members of the cluster.

```YAML
glusterfs_distributed_volumes:
  - name: vol1
    bricks: /bricks/brick1/gv1
    cluster:
      - gfs1
      - gfs2
  - name: vol2
    bricks: /bricks/brick2/gv2
```

- glusterfs_georeplication_volumes

List of configurations for the georeplications volumes.

```YAML
glusterfs_georeplication_volumes:
  - master_volume: vol1
    slave_volume: vol1
  - master_volume: vol2
    slave_volume: vol2
```

- glusterfs_georeplication_user

User to use for the georeplication, defaulted to geoaccount.    

```YAML
glusterfs_georeplication_user: replication_user
```

- glusterfs_georeplication_group

Group to use for the georeplication, defaulted to geogroup.

```YAML
glusterfs_georeplication_group: replication_group
```

- glusterfs_georeplication_mountbroker_path

Path to use for the mountbroker on the slaves, defaulted to /var/mountbroker-root.

```YAML
glusterfs_georeplication_mountbroker_path: /data/mountbroker/
```

- glusterfs_ctdb_install

A boolean added by security, need to be set to true for ctdb configuration to happen.

```YAML
glusterfs_ctdb_install: true
```

- glusterfs_ctdb_config_volume

The name of the glusterfs volume to use for storing ctdb configurations.

```YAML
glusterfs_ctdb_config_volume: volume_ctdb
```

- glusterfs_ctdb_nodes

A list of IPs of the nodes used in the ctdb configuration. This will build the /etc/ctdb/nodes files.

```YAML
glusterfs_ctdb_nodes:
  - 192.168.1.10
  - 192.168.1.11
```

- glusterfs_ctdb_public_addresses

A list of hash of the IPs and nic used in the ctdb configuration. This will build the /etc/ctdb/public_addresses files.

```YAML
glusterfs_ctdb_public_addresses:
  - ip: 192.168.1.20/24
    nic: eth0
  - ip: 192.168.1.21/24
    nic: eth0
```

- glusterfs_ctdb_shares_volume

The name of the glusterfs volume to use for hosting the ctdb shares. It will be mount in /gluster/shares folder.

```YAML
glusterfs_ctdb_shares_volume: volume_shares
```

- glusterfs_ctdb_shares:

A list of hash containing the cifs shares information. Only name and valid_users are required, defaults values are :

```YAML
force_user: nobody
force_group: nobody
writable: yes
comment: "Created by Ansible."
```
The shares will be hosted in /gluster/shares/SHARE_NAME.

```YAML
glusterfs_ctdb_shares:
  - name: share1
    valid_users:
      - DOMAIN/user
      - @DOMAIN/group
    writable: no
    force_user: nobody
    force_group: nobody
  - name: share2
    valid_users:
      - sjobs
      - bgates
```

- glusterfs_ctdb_samba_shares_config_path

The path where the shares configuration will be deployed, in order to be included inside the smb.conf. Defaulted to /etc/samba/shares.conf.

```YAML
glusterfs_ctdb_samba_shares_config_path: /path/to/store/shares.conf
```

- glusterfs_manage_repository

If you are in an environnment where packages are managed by an another role or tool, you can set this to 'no', the role will not add the official public mirror.
You will have to configure a repository before running this role.

```YAML
glusterfs_manage_repository: no
```

- glusterfs_manage_users

If you are in an environement where users are managed by an another role or tool, you can set this to 'no', the role will not try to create the replication user.
It will only generate the key on the master and push it to the first slave, you will have to create the user and allow it to ssh before running this role.

```YAML
glusterfs_manage_users: no
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
