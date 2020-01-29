# Ansible Roles 

This is a collection of ansible roles used to manage various things that
have popped into my universe over the years. They are meant to be used
as "building blocks" in a number of my own playbooks.

## Using these roles

These roles are meant to be composed in playbooks. In my own usage, I
have this centralized collection of roles, as well as a separate
centralized playbook repository that leverages these building block
roles via symlinking the repo. 

My individual projects that use these centralized plays can then build
on them with their own local plays. For example, a centralized playbook
that applies the following:
1. Initial user setup
2. Baseline security 
3. Nginx 

will still need to have nginx configuration deployed (sites-available,
etc), as well as actual www data. That is handled at a level local to
the specific project. 

Breaking things up to that level adds some complexity, but allows me to
centralize the "big" / re-usable stuff.

### "installing" these roles 

Using these roles is as simple as cloning the repository somewhere, then
creating a symlink to the [roles directory](./roles) `roles` in the folder
holding your playbooks. By symlinking the roles directory in your playbook
directory, you can emulate [the Role directory
structure](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html#role-directory-structure),
allowing your ansible playbooks to find everything they need.

If you do not have a similar structure, the roles can be cloned and your
playbooks can be updated to source the roles from a specific path.

## Available Roles

1. [Initial User Setup](./roles/initial_user) 

	The purpose of this role is to set up a user / authorized key pair
	that your control node will leverage in subsequent plays. This needs
	to be run first to ensure your control node can connect to managed
	nodes. 

2. [Baseline Security](./roles/baseline_security)

	Disables password logins, root logins, installs ufw, and configures
	ufw to only allow ssh traffic.
	
3. [Nginx](./roles/nginx)

	Installs nginx.
	
4. [Certbot](./roles/certbot)

	Installs certbot.
	
5. [Docker Compose](./roles/docker_compose)
   
   Installs docker and its prerequisites. Also installs the
   docker-compose tool. Useful in special situations where running a
   docker compose stack on a single host is fine.

