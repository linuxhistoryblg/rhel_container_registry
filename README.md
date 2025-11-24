RHEL Container Registry v1.0.0
=========

This role installs and configures a self-hosted stateless container registry to run on a RHEL8 or later server for use as a dev tool. This container registry should not be used for production purposes.

This role provides an Ansible implementaiton of the manual steps detailed in the RedHat blog post ["How to implement a simple personal/private Linux container image registry for internal use"](https://www.redhat.com/en/blog/simple-container-registry) by Stephen Wilson. Instructions for how to login to the registry as well as how to push and pull container images to and from the registry are provided in Wilson's post.

The container registry itself is provided by the [Distribuiton container image.](https://distribution.github.io/distribution/)

Requirements
------------

Defined in collections/requirements.yml
```
  collections:
    - name: ansible.posix
    - name: containers.podman
```


Role Variables
--------------

A username/password combo is created using the default values in defaults/main.yml

    # Name of your registry user
    registryuser: yourname

    # Password for your registry user
    registryuserpassword: Password123456

The values should be overridden in your calling playbook as demonstrated in the *Example Playbook* section below. 

You *could* vault the password if you really wanted to, but this registry is a dev-tool and should not be used for production purposes.

Example Playbook
----------------

Apply the rhel_container_registry role with a driver playbook like the one listed below. Specify 'startup' or 'teardown' for mode in the vars section to provision or de-provision the container registry. Override the registryuser/registryuserpassword variables with a username and password of your choice. Again, you can vault this password if cleartext passwords make you feel funny, but you should be the only user of this registry since it's only a dev tool.

```
- name: Apply the rhel_container_registry role
  hosts: target-rhel8-or-later-server-hostname
  gather_facts: true
  vars:
    # Set mode to 'startup' to install,configure,start container registry
    # Set mode to 'teardown' to stop and remove container registry
    mode: startup

    registryuser: yourname
    registryuserpassword: Password123456

  tasks:
  - name: Import the rhel_container_registry role
    ansible.builtin.import_role:
      name: rhel_container_registry
```
License
-------

GPL 3.0

Author Information
------------------

Dan Davis, dan@linuxhistory.com
