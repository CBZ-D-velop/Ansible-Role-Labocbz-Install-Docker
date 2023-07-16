# Ansible role: labocbz.install_docker

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Docker](https://img.shields.io/badge/Tech-Docker-orange)
![Tag: Docker-ce](https://img.shields.io/badge/Tech-Docker--ce-orange)
![Tag: Docker-compose](https://img.shields.io/badge/Tech-Docker--compose-orange)
![Tag: Cron](https://img.shields.io/badge/Tech-Cron-orange)

An Ansible role to install and confgure Docker and docker-compose on your hosts.

The Ansible role installs Docker and Docker Compose on the target system, enabling efficient container management. Docker Compose is installed by default, and administrators have the option to specify the desired Docker and Docker Compose versions.

For enhanced container maintenance, the role provides the flexibility to set up a cron job for periodically pruning Docker resources. This job efficiently removes unused networks and stopped or untagged images, optimizing resource utilization.

To ensure continuous availability of Docker services, the role allows configuring a cron job that restarts the Docker service after a specified time upon system boot.

Moreover, for scenarios requiring the use of private or insecure registries, the role supports configuring a list of "insecure" registries. This functionality permits the use of Artifactory, Nexus, or similar registries without requiring HTTPS for communication.

By deploying Docker and Docker Compose with this role, administrators can effectively manage containers and streamline container orchestration. The role's versatility in version selection, cron job scheduling for pruning and service restart, and support for insecure registries provides administrators with a powerful solution for containerized application deployment.

In summary, the Docker role simplifies the installation and configuration of Docker and Docker Compose. With customizable options for version selection, cron job scheduling for pruning and service restart, and support for insecure registries, the role facilitates container management and ensures a robust and efficient container environment for applications.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
docker_cron_job_weekday: "*"
docker_cron_job_minute: "*"
docker_cron_job_hour: "*/24"

docker_create_prune_cron_job: true

docker_create_cron_start_at_boot: true
docker_restart_after_boot_time: 10

docker_compose_version: "2.17.2"

#docker_insecure_registries:
#  - http://your.personnal.registrie:5049
#  - http://your.personnal.registrie:5050

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---

inv_docker_cron_job_weekday: "*"
inv_docker_cron_job_minute: "*"
inv_docker_cron_job_hour: "*/24"

inv_docker_create_prune_cron_job: true

inv_docker_create_cron_start_at_boot: true
inv_docker_restart_after_boot_time: 10

inv_docker_compose_version: "2.17.2"

inv_docker_insecure_registries:
  - http://your.personnal.registrie:5049
  - http://your.personnal.registrie:5050

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_docker"
    tags:
    - "labocbz.install_docker"
    vars:
    docker_cron_job_weekday: "{{ inv_docker_cron_job_weekday }}"
    docker_cron_job_minute: "{{ inv_docker_cron_job_minute }}"
    docker_cron_job_hour: "{{ inv_docker_cron_job_hour }}"
    docker_create_prune_cron_job: "{{ inv_docker_create_prune_cron_job }}"
    docker_create_cron_start_at_boot: "{{ inv_docker_create_cron_start_at_boot }}"
    docker_restart_after_boot_time: "{{ inv_docker_restart_after_boot_time }}"
    docker_compose_version: "{{ inv_docker_compose_version }}"
    docker_insecure_registries: "{{ inv_docker_insecure_registries }}"
    ansible.builtin.include_role:
    name: "labocbz.install_docker"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
