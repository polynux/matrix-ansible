# Ansible Playbook for Matrix and Caddy

This repository contains an Ansible playbook for setting up a Matrix homeserver with Synapse and Caddy as a reverse proxy on AlmaLinux (possibly Debian).

## File Structure

```
.
├── ansible.cfg
├── inventory
├── playbook.yaml
├── roles
│   ├── caddy
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       └── Caddyfile.j2
│   ├── docker
│   │   └── tasks
│   │       └── main.yml
│   ├── synapse
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       └── homeserver.yaml.j2
│   └── utils
│       └── main.yml
├── templates
│   ├── resolv.conf.j2
│   └── resolved.conf.j2
└── vars
└── main.yml
```

## Variable Management

We use a combination of variable files to manage configuration:

1. `vars/main.yml`: Contains common, non-sensitive variables. This file is committed to the repository.
2. `vars/secrets.yml`: Contains sensitive variables (passwords, keys). This file is not committed and should be added to `.gitignore`.
3. `vars/secrets.yml.example`: A template for `secrets.yml` with dummy values. This file is committed as a reference.

## Overriding Variables

Variables can be overridden in several ways, listed in order of increasing precedence:

1. Group Variables: Create files in a `group_vars` directory.
2. Host Variables: Create files in a `host_vars` directory.
3. Override Variables: Copy the `override.yml.example` file in the `vars` directory.
4. Command Line: Override variables when running the playbook.

Example of overriding a variable on the command line:

```bash
ansible-playbook playbook.yaml -e "matrix_domain=newdomain.com"
```

## Setting Up

1. Copy vars/secrets.yml.example to vars/secrets.yml
2. Fill in the actual secret values in vars/secrets.yml
3. Adjust variables in vars/main.yml as needed
4. Run the playbook:
```bash
ansible-playbook -i inventory playbook.yaml
```

## Notes

* This playbook is designed for AlmaLinux (and some part to Debian) but can be adapted for other distributions.
* Ensure all necessary roles and collections are installed before running the playbook.
* Review and adjust firewall and SELinux settings as needed for your environment.

For more detailed information on Ansible variable precedence and best practices, refer to the [Ansible documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html).
