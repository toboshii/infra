# k3s-homeops-ansible

This is an opinionated way to provision Ubuntu 20.04 and install k3s on top.

## Prerequisites

There's a couple things that will need to be done before you get starting running with Ansible.

1) Install Ansible >= 2.10.0 on your local machine
2) Install a supported OS on each of your nodes
3) Set static IP for each node on the OS or in your router (you can use the IP assigned via DHCP, but it's not recommended)
4) Copy your local public ssh key with `ssh-copy-id` to each node
5) _Optional_ review playbooks and roles to understand what these Ansible script will do

After that we're ready to continue with Ansible...

## Ansible

Get started by cloning this repository and copying the hosts and config into a new directory.

```bash
# clone this repo
git clone https://github.com/onedr0p/k3s-homeops-ansible
# change into the directory
cd k3s-homeops-ansible
# copy the hosts and config to a new folder
cp -r ./inventory/local ./inventory/custom
```

### Update the Ansible config files

**Note:** This project uses [PyratLabs/ansible-role-k3s](https://github.com/PyratLabs/ansible-role-k3s) for installing k3s. Configuration options can be viewed in their README.

After you have copied over the configuration files you will need to update the configuration in the files:

- `./inventory/custom/hosts.yml`: Host definitions
- `./inventory/custom/host_vars/*.yml`: Host IP and host level variables
- `./inventory/custom/group_vars/*.yml`: Global variables for all hosts

Each file it carefully documented.

### Get Ansible dependencies

```bash
ansible-galaxy install -r requirements.yml
```

### Run the playbooks

```bash
# This playbook will prepare your nodes for Kubernetes
ansible-playbook -i ./inventory/custom/hosts.yml ./playbooks/os-build.yml
# This playbook will install k3s
ansible-playbook -i ./inventory/custom/hosts.yml ./playbooks/cluster-build.yml
```

### Verify the cluster is up and running

```bash
kubectl --kubeconfig ./kubeconfig get nodes -o wide
```
