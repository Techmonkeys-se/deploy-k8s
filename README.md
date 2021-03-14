# Ansible Roles to deploy multi-master Kubernetes on Redhat Linux 8
=========

Collection of Ansible roles to setup servers and deploy a multi-master Kubernetes. Using containerd.io

- Deployment of masters & workers.
- set up networking plug-in via yaml file provided url/location by user
- Ecnrypt secrets at rest
- Ingress deplyment.

## Requirements
------------

A deploy server with RHEL 8+ with ansible installed, a user with ssh-keys and ssh-key access to all nodes and sudo access (need flags "--become-user root --ask-becom-pass") or ssh as root directly where you are going to deploy k8s on.

## Preparations
--------------

- Clone the repo to the machine which you are going to use as bastion/deploy host.

´´´
git clone deploy-k8s CHANGE URL!
cd deploy-k8s
´´´

- Edit the inventory (inventory/k8shosts)

- Update all variables to suit your deployment
´´´nano group_vars/all.yml´´´

## Install Kubernetes
------------

´´´
ansible-playbook -i inventory/k8shosts deploy-k8s.yml
´´´

Will init the first master and the rest of the servers you have configured in hosts file.

## Work in progress
----------------

This collection is a work in progress i use to learn ansible and kubernetes.

## Todo list
----------------

- Clean up script
- add flux, add backup
- add storage (Longhorn?)
- tag tasks in roles?

## Contribution
------------------

Feel free to create issues here on github and ill go thru all when i can or help out to add features.

Special thanks to [Schabo](https://github.com/Schabo) for the massive help in ideas and troubleshooting.
