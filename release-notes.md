# Release notes

Here i try to mention bigger changes made to these Ansible roles.

## 2021-05-08
------------
- Removed `kubeadm reset -f` command when running deploy-k8s.yml. Now you weill need to manually run kill-cluster.yaml inorder to reset your cluster if need to re-init.

## 2021-05-07
------------
- Clean up of all tasks
- All tasks are ansible modules now