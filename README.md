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

`roles` is a reserved word: `[WARNING]: Found variable using reserved name: roles`. This variable name will change in future versions, probably a breaking change.

## Examples

### Project

    /tmp/ansible-project/
    ├── database.yml
    ├── group_vars
    ├── host_vars
    ├── library
    ├── master.yml
    ├── production
    ├── roles
    │   ├── database
    │   │   ├── defaults    
    │   │   ├── files
    │   │   ├── handlers
    │   │   │   └── main.yml
    │   │   ├── meta
    │   │   ├── tasks
    │   │   │   └── main.yml
    │   │   ├── templates
    │   │   └── vars
    │   └── web
    │       ├── defaults        
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
### Version 0.1.0 – May 23, 2017
  - Added `library` to main directory and `defaults` to role directories
  - Added default `group_vars/all` file
  - Fixed YAML syntax to quote strings and use colon separators for attributes
  - Fixed unquoted `{{ roles }}` variable in [roles/init/tasks/main.yml](roles/init/tasks/main.yml. This was breaking role creation in newer versions of ansible.

## License

MIT: http://davestern.mit-license.org
