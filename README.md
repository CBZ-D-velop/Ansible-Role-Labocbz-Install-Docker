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
![Tag: Portainer](https://img.shields.io/badge/Tech-Portainer-orange)
![Tag: Watchtower](https://img.shields.io/badge/Tech-Watchtower-orange)

An Ansible role to install and confgure Docker and docker-compose on your hosts.

The Ansible role installs Docker and Docker Compose on the target system, enabling efficient container management. Docker Compose is installed by default, and administrators have the option to specify the desired Docker and Docker Compose versions.

When initiating a fresh Docker installation, consider the advantages of integrating Portainer and Watchtower. Portainer offers an intuitive, graphical interface that simplifies container management, ideal for those new to Docker. Including Portainer during setup grants immediate visual control over clusters, images, and networks.

Complementing this, Watchtower automates container updates. With Watchtower integrated into your Docker ecosystem, you ensure that your containers are consistently up-to-date. It monitors and installs new versions, ensuring security and optimal performance. Together, Portainer and Watchtower enhance your Docker experience by providing user-friendly management and automated updates for a seamless container journey.

Moreover, for scenarios let you use or not of private or insecure registries, the role supports configuring a list of "insecure" registries. This functionality permits the use of Artifactory, Nexus, or similar registries without requiring HTTPS for communication.

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
install_docker__compose_version: "2.17.2"

install_docker__disable_swap: false

#install_docker__insecure_registries:
#  - http://your.personnal.registrie:5049
#  - http://your.personnal.registrie:5050

install_docker__portainer: true
install_docker__portainer_http_port: 9000
install_docker__portainer_https_port: 9443
install_docker__portainer_agent_port: 8000
install_docker__portainer_address: "0.0.0.0"
install_docker__portainer_container_name: "portainer-ce"
install_docker__portainer_volume_name: "portainer_data"
install_docker__portainer_ssl: true
install_docker__portainer_ssl_path: "/etc/certs"
install_docker__portainer_ssl_cert: "{{ install_docker__portainer_ssl_path }}/mycert.crt"
install_docker__portainer_ssl_key: "{{ install_docker__portainer_ssl_path }}/mykey.key"

install_docker__watchtower: true
install_docker__watchtower_container_name: "watchtower"
install_docker__watchtower_poll_interval: 3600
install_docker__watchtower_cleanup: true
install_docker__watchtower_include_restarting: true
install_docker__watchtower_include_stopped: true
install_docker__watchtower_no_restart: true

install_docker__handle_clean: true
install_docker__clean_cron_file: "ansible_docker_system_prune"
install_docker__clean_weekday: "*"
install_docker__clean_minute: "*"
install_docker__clean_hour: "*/8"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_docker__compose_version: "2.17.2"

inv_install_docker__disable_swap: true

#inv_install_docker__insecure_registries:
#  - http://your.personnal.registrie:5049
#  - http://your.personnal.registrie:5050

inv_install_docker__portainer: true
inv_install_docker__portainer_http_port: 9000
inv_install_docker__portainer_https_port: 9443
inv_install_docker__portainer_agent_port: 8000
inv_install_docker__portainer_address: "127.0.0.1"
inv_install_docker__portainer_container_name: "portainer-ce"
inv_install_docker__portainer_volume_name: "portainer_data"
inv_install_docker__portainer_ssl: true
inv_install_docker__portainer_ssl_path: "/etc/docker/ssl/portainer"
inv_install_docker__portainer_ssl_cert: "{{ inv_install_docker__portainer_ssl_path }}/my-portainer-server.domain.tld/my-portainer-server.domain.tld.pem.crt"
inv_install_docker__portainer_ssl_key: "{{ inv_install_docker__portainer_ssl_path }}/my-portainer-server.domain.tld/my-portainer-server.domain.tld.pem.key"

inv_install_docker__watchtower: true
inv_install_docker__watchtower_container_name: "watchtower"
inv_install_docker__watchtower_poll_interval: 3600
inv_install_docker__watchtower_cleanup: true
inv_install_docker__watchtower_include_restarting: true
inv_install_docker__watchtower_include_stopped: true
inv_install_docker__watchtower_no_restart: true

inv_install_docker__handle_clean: true
inv_install_docker__clean_cron_file: "ansible_docker_system_prune"
inv_install_docker__clean_weekday: "*"
inv_install_docker__clean_minute: "*"
inv_install_docker__clean_hour: "*/8"
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
    install_docker__portainer: "{{ inv_install_docker__portainer }}"
    install_docker__portainer_http_port: "{{ inv_install_docker__portainer_http_port }}"
    install_docker__portainer_https_port: "{{ inv_install_docker__portainer_https_port }}"
    install_docker__portainer_agent_port: "{{ inv_install_docker__portainer_agent_port }}"
    install_docker__portainer_container_name: "{{ inv_install_docker__portainer_container_name }}"
    install_docker__portainer_volume_name: "{{ inv_install_docker__portainer_volume_name }}"
    install_docker__portainer_ssl: "{{ inv_install_docker__portainer_ssl }}"
    install_docker__portainer_ssl_volume_name: "{{ inv_install_docker__portainer_ssl_volume_name }}"
    install_docker__portainer_ssl_key: "{{ inv_install_docker__portainer_ssl_cert }}"
    install_docker__watchtower: "{{ inv_install_docker__watchtower }}"
    install_docker__watchtower_poll_interval: "{{ inv_install_docker__watchtower_poll_interval }}"
    install_docker__watchtower_cleanup: "{{ inv_install_docker__watchtower_cleanup }}"
    install_docker__watchtower_include_restarting: "{{ inv_install_docker__watchtower_include_restarting }}"
    install_docker__watchtower_include_stopped: "{{ inv_install_docker__watchtower_include_stopped }}"
    install_docker__watchtower_no_restart: "{{ inv_install_docker__watchtower_no_restart }}"
    install_docker__portainer_address: "{{ inv_install_docker__portainer_address }}"
    install_docker__handle_clean: "{{ inv_install_docker__handle_clean }}"
    install_docker__clean_cron_file: "{{ inv_install_docker__clean_cron_file }}"
    install_docker__clean_weekday: "{{ inv_install_docker__clean_weekday }}"
    install_docker__clean_minute: "{{ inv_install_docker__clean_minute }}"
    install_docker__clean_hour: "{{ inv_install_docker__clean_hour }}"
    install_docker__disable_swap: "{{ inv_install_docker__disable_swap }}"
  ansible.builtin.include_role:
    name: "labocbz.install_docker"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-08-28: Portainer / Watchtower

* Role can install Portainer
* Role can install Watchtower
* No more "special workaround"
* No more cron purne job

### 2023-09-05: Portainer SSL support

* Role can now deploy Portainer with custom SSL

### 2023-09-05b: Watchtower env support

* You can now set some env vars for Watchtower like poll interval or cleanup

### 2023-09-27: Fix expose port and volumes

* Role can now start containers
* Role allow yo to expose Portainer on localhost if you want any reverse proxy

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-11-13: Agent port

* Role bind agent port on 0.0.0.0, so bind on 127.0.0.1 is now possible for security

### 2023-12-04: Docker cleaning

* Role can now create / remove cron task to clean / purge the unused objects

### 2023-12-29: Daemon.json

* Daemon file created before install so Docker daemon start with correct params directly

### 2023-12-30: SWAP

* You can now disable your SWAP or enable it with this role
* SWAP disabing is based on cron task at boot

### 2024-02-24: Fix and CI

* Added support for new CI base
* Edit all vars with __
* Tested and validated on Docker DIND
* Removed docker socket

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
