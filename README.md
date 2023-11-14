Requirements
------------

You must specifiy minio_disks variable

```yaml
minio_disks:
  - { disk: '/dev/vdb', fstype: 'xfs' }
  - { disk: '/dev/vdc', fstype: 'xfs' }

```

Set a list of nodes to create a [distributed cluster (Multi-Node Multi-Drive deployment)](https://min.io/docs/minio/linux/operations/install-deploy-manage/deploy-minio-multi-node-multi-drive.html).
```yaml
minio_nodes: "minio0{1...2}"

```
or 

```yaml
minio_nodes: "minio0{1...{{ansible_play_hosts | length}}}"

```

Example Playbook
----------------


```yaml
---
- name: Instal MinIO
  hosts: minio

  tasks:

  - include_role: name=ansible-role-minio
    vars:
      minio_nodes: "minio0{1...{{ansible_play_hosts | length}}}"
      minio_disks:
        - { disk: '/dev/vdb', fstype: 'xfs' }
        - { disk: '/dev/vdc', fstype: 'xfs' }
      minio_buckets:
      - bucket1
      - bucket2
      minio_users:
      - name: 'user1'
        password: 'OeHa5ohquoh8een9'
        policies: ['read-write_bucket1']
      - name: 'user2'
        password: 'Iwohh3ieziep2wii'
        policies: ['read-only_bucket2']
      - name: 'user3'
        password: 'Efoo7hoozi9oom0b'
        policies: ['read-write_bucket1', 'read-write_bucket2']
```

Test
----------------


```bash
cd tests

vagrant up

vagrant destroy -f
```