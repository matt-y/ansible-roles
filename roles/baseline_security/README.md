# Initial security practices for a new Ubuntu remote 

This role sets up a baseline of security on a remote. 

It will disable root logins, disable all password logins, install ufw,
and finally create a ufw rule that only allows ssh connections.

## Prerequisites

1. The [initial_user](../initial_user) role having been applied. The
   `initial_user` role ensures we have a user set up so that when the
   `baseline_security` role locks down ssh, playbooks will continue to
   be runnable with the `user_name` that the `initial_user` role was
   applied with. See [initial_user](../initial_user) for
   more information about why that is important.

## Variables that need to be provided

None. 



