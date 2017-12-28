docker
========

Installs Docker on:

* Ubuntu 16.04
* Debian 9
* CentOS 7

**Example Play**:

Very basic install utilizing the role defaults:

```
---
- name: Run docker
  hosts: docker
  roles:
    - basecomponent.docker
```

Overriding the default configration is done by overriding the role's default variables:

```
- name: Install Docker
  hosts: all
  vars:
    docker_directory: "/data/lib/docker"
  roles:
    - role: basecomponent.docker
```


Role Variables
--------------

Please see [defaults/main.yml](https://github.com/basecomponent/docker/blob/master/defaults/main.yml) for a comprehensive list of variables that can be overridden.

Dependencies
------------

None.

Testing
-------

To test the role in a Vagrant environment just run `vagrant up`.  This will
create some VMs:

* Ubuntu 16.04
* Debian Stretch 9.0
* CentOS 7
* SLES   12sp3

and it will provision them by applying this role with Ansible.

Requires `ansible-playbook` to be in the path.

License
-------

Apache v2.0
