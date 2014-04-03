ansible-init
=========================

an ansible playbook for creating ansible playbooks.

Ansible playbook to set up one common ansible project structure.

See:
http://docs.ansible.com/playbooks_best_practices.html
and
http://docs.ansible.com/playbooks_roles.html

Usage:

The project will be build locally by default.

    ansible-playbook -i production init.yml --connection=local --extra-vars '{"roles": ["web", "database", "etc"], "project_dir": "/home/user/ansible/project"}'

OR using a JSON file with variables:

    ansible-playbook -i production init.yml --connection=local --extra-vars "@inifile.json"

JSON file:

    {
        "roles": [
            "web",
            "database",
            "etc"
        ],
        "project_dir": "/home/user/ansible/project"
    }

roles: all the roles that should be created
project_dir: the location to create ansible project

To run on a remote server, remove `--connection=local` from the CLI and change the `production` inventory file.
