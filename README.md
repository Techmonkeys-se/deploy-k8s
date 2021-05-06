# Ansible Roles to deploy multi-master Kubernetes on Redhat Linux 8
=========

Collection of Ansible roles to setup servers and deploy a multi-master Kubernetes. Using containerd.io

- Deployment of masters & workers.
- set up networking plug-in via yaml file provided url/location by user
- Ecnrypt secrets at rest
- Ingress deplyment. (haproxy)
- Add new master/worker nodes

## Requirements
------------

A deploy server with RHEL 8+ with ansible installed, a user with ssh-keys and ssh-key access to all nodes as root user. These playbooks are only tested and written to be run as root directly towards the remote server.

## Preparations
--------------

- Clone the repo to the machine which you are going to use as bastion/deploy host.

```git clone deploy-k8s CHANGE URL!
cd deploy-k8s
```

- install kubectl on server you deploy from, read here: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

- Edit the inventory (inventory/k8shosts)

- Update all variables to suit your deployment
´´´nano group_vars/all.yml´´´

- Update your internal dns to point to each server hostname & and api_dns, dns name in your LB/router that points to all nodes running API

## Install Kubernetes
------------

`ansible-playbook -i inventory/k8shosts deploy-k8s.yml`
or
`ansible-playbook -i inventory/k8shosts deploy-k8s.yml --become-user root --ask-become-pass`

Will init the first master and the rest of the servers you have configured in hosts file.

## Adding a node
- Add the node to inventory/k8shosts correct group
- run 
    `ansible-playbook -i inventory/k8shosts add-node.yml --limit "node-fqdn"`

You can always run: 
`ansible-playbook -i inventory/k8shosts add-node.yml --limit "node-fqdn -C` to do a dry-runbefore deploying so you see the correct hosts are being provisioned

## Work in progress
----------------

This collection is a work in progress i use to learn ansible and kubernetes.

## Todo list
----------------

- Clean up script
- add RBAC config?
- add flux, add backup
- add storage (Longhorn?)
- security (stackrox?)
- monitoring (prometheus, grafana, telegraf?)
- tag tasks in roles?

## Good to know, Known issues
----------------

- Sometimes kubelet fails to start after join, more information:
https://github.com/kubernetes/kubernetes/issues/99305

- If you ever change networking plugin you need to clean up /etc/cni/net.d/ folder from old cni configs.

- Calico network plugin is not plug and play. You have been warned (or im too stupid to learn BGP)

- Storage role with Longhorn has some issues and need manual work
  - the yaml file need to be applied several times for all settings to be implemented

## Contribution
------------------

Feel free to create issues here on github and ill go thru all when i can or help out to add features.

Special thanks to [Schabo](https://github.com/Schabo) for the massive help in ideas and troubleshooting.
