# Prepare Jenkins, Docker and Kubectl in CentOS 9

An Ansible playbook to prepare Jenkins server, Docker platform, and Kubectl for Kubernetes cluster.

## Pre-requisites

Prepare an Ansible connection from the control node to managed node using [Ansible setup](./pre-requisites/Ansible-setup-in-CentOS-9.md) document.

### Prepare kubernetes cluster

To maintain a separate Kubernetes cluster from the Jenkins server and execute commands using `kubectl`, you can follow the instructions in the [kubernetes-cluster-setup-playbook](https://github.com/mohammadrony/kubernetes-cluster-setup-playbook.git) repository to create a cluster with a master and worker node.

## Clone this repository to ansible home

```bash
git clone https://github.com/mohammadrony/<jenkins-docker-kubectl-install>.git
```

## Update variable in playbook

Update the control plane ip address variable [file](./kube-control-setup/vars/main.yml) file in the repository.

## Check ansible hosts file

Check [hosts](./hosts) file for host-group and host-names with control plane and worker node host names. By default it would work as follows

```bash
[admin]
admin-server
```

## Finally run the ansible-playbook as ansible user

```bash
su ansible
ansible-playbook playbook.yml
```

Thank you.
