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

In the [ansible best practices](http://docs.ansible.com/playbooks_best_practices.html) the inventory file is called `production`. I chose to follow this convention for the inventory of **ansible-init** itself. If you prefer a different name for the **ansible-init** inventory (like `inventory`), just change the name of the file and run the `ansible-playbook` with `-i inventory` instead. If you prefer different inventory files for your generated project, update the list in the `"Create inventory files"` task in [roles/init/tasks/main.yml](roles/init/tasks/main.yml)

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

    - include: web.yml
    - include: database.yml


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
