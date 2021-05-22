![Kubernetes with Ansible](https://github.com/Edd1e360/deploy-k8s/blob/main/ansible-kubernetes.jpg)
# Ansible Roles to deploy Kubernetes on Redhat Linux 8

Collection of Ansible roles to setup servers and deploy a multi-master Kubernetes. Using containerd.io

- Deployment of masters & workers.
- set up networking plug-in with flannel
- Encrypt secrets at rest
- Ingress deployment. (haproxy) This is optional, need to remove commented line
- Add new master/worker nodes
- Kill cluster (if in need of a reset)
## Purpose of this project / History for starting this project
------------
Before i started this project i tried a lot of different deployment script but of those that worked did not have fully functioning cluster without manual work after deployment. Looking at other projects i wanted some parts that i wanted. So i started learning from then and writing this repository.

The goal is to have a deployment script that deploys a fully functional production ready kubernetes cluster. A lot of it should be customizable to ones requirments.
## Requirements
------------
- A deploy server with RHEL 8+ with ansible installed
- User with ssh-keys and ssh-key access to all nodes as root user. 
- All nodes need atleast 2cpu 4gig ram (bare minimum)

These playbooks are only tested and written to be run as root directly towards the remote server.
## Preparations
--------------
- Clone this repo to the machine which you are going to use as deploy host.

- Install kubectl on server you deploy from (OPTIONAL), read here: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

- Edit the inventory (inventory/k8shosts)

- Update all variables to suit your deployment

```
cp group_vars/all.yml.example group_vars/all.yml
nano group_vars/all.yml
```

- Update your internal dns to point to each server & and api_dns, dns name in your LB/router that points to all nodes running API
## Install Kubernetes
------------

Remember, Running the deploy-k8s.yml playbook will destroy a cluster if previously installed on hosts in inventory

`ansible-playbook -i inventory/k8shosts deploy-k8s.yml`

This playbook will installed all dependencies and requirerments. Then init the cluster.
After installation is done `kubectl get nodes` will fail for a while because its restarting kubelet as a last step
## Adding a node
------------
- Add the node to inventory/k8shosts correct group
- run: `ansible-playbook -i inventory/k8shosts add-node.yml --limit <FQDN>`
  
  replace `<FQDN>` with full hostname

You can always run the playbook with `-C` for a dry-run to see changes that will happen
## Work in progress
----------------
This collection is a work in progress i use to learn ansible and kubernetes. Script has been tested on 3 master/3 worker setup and 1master/1worker.

## Todo list
----------------

- ~~Clean up playbooks~~
- Add RBAC config
- Add flux (create gitrepo with template files for cluster bootstrapping)
- Add backup script
- add storage (via flux...)
- security (via flux)
- monitoring (prometheus, grafana, telegraf?) (add this in flux git)
- tag tasks in roles?

## Good to know, Known issues
----------------
- Sometimes kubelet fails to start after join, more information:
https://github.com/kubernetes/kubernetes/issues/99305
- If you ever change networking plugin you need to clean up /etc/cni/net.d/ folder from old cni configs.
- Storage role with Longhorn has some issues and need manual work (therefore disabled by default)
  - The yaml file need to be applied several times for all settings to be implemented

## Contribution
------------------
Feel free to create issues here on github and i´ll go thru all when i can or help out to add features.

Special thanks to [Schabo](https://github.com/Schabo) for the massive help in ideas and troubleshooting.

2021-05-22 Transfered ownership to techmonkeys.se (Techmonkeys-se).
