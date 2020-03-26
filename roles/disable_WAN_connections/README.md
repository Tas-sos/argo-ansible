Disable WAN interface.
============================

Take a network interface down.
The purpose of this role is to disable the interface which is responsible for internet access.

Requirements
------------

* A another network interface with *private ip* address which will now make connections to the machine.
* A bastion/gateway machine belonging to the same private network and accessing the machine.

Role Variables
--------------

`wan_interface`: The valid network interface name of the machine which has the **public** (WAN)  IP address.

Dependencies
------------
Current connection to the machine from its local address via the bastion/gateway machine.

Example Inventory
-----------------

```yaml
cluster_01
cluster_02
cluster_03

bastion

[c_gateway]
bastion

[c_jobs]
cluster_01
cluster_02
cluster_03

[c_cluster:children]
c_gateway
c_jobs

[c_private:children]
c_jobs

```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

```yaml
- hosts:  c_private
  become: yes 
  roles:
    - { role: disable_WAN_connections }
```

Example of `group_vars/c_cluster`
---------------------------------

```yaml
---

ansible_ssh_common_args: '-o ProxyCommand="ssh -W %h:%p -q bastion"'

# ....
```

Example of `~/.ssh/config`
--------------------------

```bash
Host cluster_01
        Hostname 192.168.0.1
        User cluster_user_1
        PubkeyAuthentication yes

Host cluster_02
        Hostname 192.168.0.2
        User cluster_user_2
        PubkeyAuthentication yes 

Host cluster_03
        Hostname 192.168.0.3
        User cluster_user_3
        PubkeyAuthentication yes


Host bastion
        Hostname 123.456.789.0
        User test
        PubkeyAuthentication yes 
```



License
-------

Apache 2

Author Information
------------------

GRNET
