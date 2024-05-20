# Ansible role: labocbz.add_docker_swarm

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: Docker](https://img.shields.io/badge/Tech-Docker-orange)
![Tag: Swarm](https://img.shields.io/badge/Tech-Swarm-orange)
![Tag: Portainer](https://img.shields.io/badge/Tech-Portainer-orange)

An Ansible role to configure a Docker SWARM on your hosts.

Enhance your Docker Swarm orchestration with this Ansible role, designed to simplify the entire process. Automated procedures initiate the cluster on a manager node, generate tokens for gradual expansion with managers and workers, and streamline SSL/mTLS encryption configuration. Plus, we've seamlessly integrated a Portainer stack, offering a powerful web-based interface for convenient cluster management. Whether your focus is on rapid deployment or customized security settings, this Ansible role makes it easy with straightforward variable specifications.

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
add_docker_swarm__is_manager: false

add_docker_swarm__history_limit: 1
add_docker_swarm__ssl: false
#add_docker_swarm__ssl_verify_cert: "no"
#add_docker_swarm__tls_name: "{{ inventory_hostname }}"
#add_docker_swarm__ssl_path: "/path/to/your"
#add_docker_swarm__ca: "{{ add_docker_swarm__ssl_path }}/ca.pem.crt"
#add_docker_swarm__key: ""{{ add_docker_swarm__ssl_path }}/key.pem.crt"
#add_docker_swarm__cert: "{{ add_docker_swarm__ssl_path }}/cert.pem.crt"
add_docker_swarm__portainer_volume_name: "portainer_data"
#add_docker_swarm__portainer_https_port: 9443
add_docker_swarm__portainer_http_port: 9000

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
#inv_add_docker_swarm__is_manager: false
inv_add_docker_swarm__history_limit: 1
inv_add_docker_swarm__ssl: true
inv_add_docker_swarm__ssl_verify_cert: "no"
inv_add_docker_swarm__ssl_path: "/etc/docker/swarm/ssl"
inv_add_docker_swarm__tls_name: "{{ inventory_hostname }}"
inv_add_docker_swarm__ca: "{{ inv_add_docker_swarm__ssl_path }}/ca-chain.pem.crt"
inv_add_docker_swarm__key: "{{ inv_add_docker_swarm__ssl_path }}/my-docker-swarm-cluster.domain.tld.pem.key"
inv_add_docker_swarm__cert: "{{ inv_add_docker_swarm__ssl_path }}/my-docker-swarm-cluster.domain.tld.pem.crt"
inv_add_docker_swarm__portainer_volume_name: "portainer_data"
inv_add_docker_swarm__portainer_https_port: 9443
inv_add_docker_swarm__portainer_http_port: 8000

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.add_docker_swarm"
  tags:
    - "labocbz.add_docker_swarm"
  vars:
    add_docker_swarm__is_manager: "{{ inv_add_docker_swarm__is_manager }}"
    add_docker_swarm__history_limit: "{{ inv_add_docker_swarm__history_limit }}"
    add_docker_swarm__ssl: "{{ inv_add_docker_swarm__ssl }}"
    add_docker_swarm__ssl_verify_cert: "{{ inv_add_docker_swarm__ssl_verify_cert }}"
    add_docker_swarm__ssl_path: "{{ inv_add_docker_swarm__ssl_path }}"
    add_docker_swarm__tls_name: "{{ inv_add_docker_swarm__tls_name }}"
    add_docker_swarm__ca: "{{ inv_add_docker_swarm__ca }}"
    add_docker_swarm__key: "{{ inv_add_docker_swarm__key }}"
    add_docker_swarm__cert: "{{ inv_add_docker_swarm__cert }}"
    add_docker_swarm__portainer_volume_name: "{{ inv_add_docker_swarm__portainer_volume_name }}"
    add_docker_swarm__portainer_https_port: "{{ inv_add_docker_swarm__portainer_https_port }}"
    add_docker_swarm__portainer_http_port: "{{ inv_add_docker_swarm__portainer_http_port }}"
  ansible.builtin.include_role:
    name: "labocbz.add_docker_swarm"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-12-24: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-12-24-b: Docker Swarm pre install

* Role deploy a Docker Swarm, but not tested in local (Docker in Docker in Docker ...)
* Need test in Labo-CBZ
* Role handle SSL / mTLS
* Role handle custom name

### 2023-12-29: Tests and fixes

* Role doesn't handle Swarm name
* Tested in develop and validation env

### 2024-02-24: Fix and CI

* Added support for new CI base
* Edit all vars with __
* Tested and validated on Docker DIND
* Removed docker socket local and port

### 2024-05-19: New CI

* Added Markdown lint to the CICD
* Rework all Docker images
* Change CICD vars convention
* New workers
* Removed all automation based on branch

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
