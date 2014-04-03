ansible-init
=========================

Ansible playbook for creating ansible playbooks.

The playbook sets up the common ansible project structure described in the [ansible best practices](http://docs.ansible.com/playbooks_best_practices.html).


## Usage:

Use either of the methods below to set up a local directory and file structure ready for `ansible-playbook` use. All that's needed is to modify the inventory, then add tasks, handlers, files and templates.

    ansible-playbook \
    init.yml \
    -i production \
    --connection=local \
    --extra-vars='{"roles": ["web", "database"], "project_dir": "/tmp/ansible-project"}'

or using a JSON file with variables:

    ansible-playbook \
    init.yml \
    -i production \
    --connection=local \
    --extra-vars='@project.json'

Default project JSON file:

    {
        "roles": [
            "web",
            "database"
        ],
        "project_dir": "/tmp/ansible-project"
    }

**roles:** array of roles that should be created

**project_dir:** the directory in which to create the ansible project

## Notes

The project will be built locally by default.

To run on a remote server, remove `--connection=local` from the CLI and change the `production` inventory file to specify the destination server.


## Examples

### Project

    /tmp/ansible-project/
    ├── database.yml
    ├── group_vars
    ├── host_vars
    ├── master.yml
    ├── production
    ├── roles
    │   ├── database
    │   │   ├── files
    │   │   ├── handlers
    │   │   │   └── main.yml
    │   │   ├── meta
    │   │   ├── tasks
    │   │   │   └── main.yml
    │   │   ├── templates
    │   │   └── vars
    │   └── web
    │       ├── files
    │       ├── handlers
    │       │   └── main.yml
    │       ├── meta
    │       ├── tasks
    │       │   └── main.yml
    │       ├── templates
    │       └── vars
    ├── staging
    └── web.yml

### Files

#### master.yml

    ---
    # master playbook

    - include: { include: web.yml }
    - include: { include: database.yml }


#### web.yml

    ---
    # web role

    - hosts: web
        roles:
        - { role: web }

#### roles/web/tasks/main.yml

    ---
    # Tasks for web

    #- name: example
    #  action: example
    #  notify:
    #    - restart service
    #  tags:
    #    - example

#### roles/web/handlers/main.yml

    ---
    # Handlers for web

    #- name: example restart
    #  action: service name=service state=restarted


## CHANGELOG

### Version 0.0.1 – April 2, 2014

  - Initial release.

## License

MIT: http://davestern.mit-license.org
