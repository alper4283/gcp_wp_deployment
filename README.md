# WordPress Deployment with Ansible (GCP)

## Overview
This repository contains an Ansible playbook to deploy a WordPress environment on Google Cloud VMs.
It automates system preparation, package installation, database setup, and WordPress configuration.

Infrastructure provisioning (VM creation, networking) is expected to be done beforehand.

## Prerequisites
- Ansible installed locally
- SSH access to target VM(s)
- sudo privileges on target VM(s)
- Python 3 installed on target VM(s)

## Inventory setup

A template inventory file is provided:

    hosts.ini.dist

Create your own inventory file:

    cp hosts.ini.dist hosts.ini

Edit hosts.ini with your VM details:

    [vms]
    wp_host ansible_host=1.2.3.4

    [vms:vars]
    ansible_user=PC-03
    ansible_python_interpreter=/usr/bin/python3

## Configuration and secrets

General variables are defined in:

    group_vars/all/main.yml

Sensitive values (encrypted with Ansible Vault) are stored in:

    group_vars/all/vault.yml

Default Ansible Vault password:

    milyedel

View or edit vault contents:

    ansible-vault view group_vars/all/vault.yml
    ansible-vault edit group_vars/all/vault.yml

## Running the playbook

Run a full deployment:

    ansible-playbook -i hosts.ini site.yml \
      --ask-become-pass \
      --ask-vault-pass

## Using tags

The system role supports the following tags:
- os_updates
- env_setup
- system

Example: run only OS updates

    ansible-playbook -i hosts.ini site.yml \
      --ask-become-pass \
      --ask-vault-pass \
      --tags os_updates

Example: skip OS updates

    ansible-playbook -i hosts.ini site.yml \
      --ask-become-pass \
      --ask-vault-pass \
      --skip-tags os_updates

## Project context
This repository provides configuration management only.
It is part of a larger platform demo that also includes infrastructure provisioning,
CI/CD pipelines, monitoring, backups, and performance testing.
