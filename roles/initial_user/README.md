# Base Linux Install: Setting up a user

This role is to set up a user / authorized key pair on a remote.

One can leverage this play to create a user that any subsequent plays
can be run _through_, allowing a consistent access point. 

## Why do I want a consistent access point?

When the role `baseline_security` is applied, it will secure a remote by
disabling all access except through ssh key authorization as a non-root
user. Therefore it is important to have created a user / key pair that
allows you to continue running plays.

A playbook can be created that applies the `initial_user` role on
several hosts, and applied to hosts first before any other plays are
run.

Other playbooks, like a playbook that installs and configures apache,
can then leverage the user that the `initial_user` role
created. If one of those plays happen to include the `baseline_security`
role then **they will remain idempotent**, because we have a non-root
user with ssh access we can continuously run them through.


## Prerequisites

1. Requires a remote host with a user that we can connect through password
   authentication over ssh. Pass this user into the playbok using `-k -u $user`.
2. Requires a public key that can be deployed to the remote machine via the
   `ssh_key` variable.

## Variables that need to be provided

A playbook installing this role will need to provide the following variables

### `user_name`

The name of the user to create on the remote machine. 

### `ssh_key`

Path to a local public key. This key will be added to the set of authorized keys
for the new user `user_name` that is created on the remote host.

## Example

Here is an example playbook 

``` yaml
---
- name: Initial user setup
  become: true
  hosts: a_host

  vars:
    user_name: not-root
    #or whatever public key you want 
    ssh_key: "{{ lookup('file', lookup('env','HOME') + '/.ssh/id_rsa.pub') }}"

  roles:
    - initial_user
```

And here is how that playbook might be applied. In this case `root` is
the system user that we are able to authenticate via password to with
_only initially_.

```
ansible-playbook basic-linux/base-install.yml -k \
-u root 
```

In subsequent playbooks, the `not-root` user will be passed via the `-u` flag:
`-u not-root`, and a connection will be made via the `ssh_key` supplied in the
initial playbook.
