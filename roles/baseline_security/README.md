# Initial security practices for a new Ubuntu remote 

This role sets up a baseline of security on a remote. 

It will disable root logins, disable all password logins, install ufw,
and finally create a ufw rule that only allows ssh
connections. Additionally, a ufw `limit` rule is created for the ssh
port, which is combined with a shorter `LoginGraceTime` value in the
`sshd_config` file to prevent potential DOS attacks.

## Prerequisites

0. You must install the community.general collection from ansible-galaxy
   to enable the `community.general.ufw` module
   
```sh
ansible-galaxy collection install community.general
```
	

1. The [initial_user](../initial_user) role having been applied. The
   `initial_user` role ensures we have a user set up so that when the
   `baseline_security` role locks down ssh, playbooks will continue to
   be runnable with the `user_name` that the `initial_user` role was
   applied with. See [initial_user](../initial_user) for
   more information about why that is important.

## Variables that need to be provided

### `ssh_port`

Specifies what port you want to try to run SSH on. The default is 22 and
is a widely known attack vector. This is a good thing to change. If the
`ssh_port` is provided, a ufw `allow` rule will be created for it. If it
is not provided a UFW rule will be created for the default port. 

### `ssh_allowed_user_list` 

If you with to limit what specific users can connect via ssh, provide
them as a list: `['user1', 'user2']` etc. 



