# Prepare Jenkins, Docker and Kubectl in CentOS 9

An Ansible playbook to prepare Jenkins server, Docker and Kubectl configuration in CentOS 9

## Pre-requisites

Prepare an Ansible connection from the control node to the managed node using the [Ansible setup](./pre-requisites/Ansible-setup-in-CentOS-9.md) document.

### Prepare kubernetes cluster

If you are to maintain a Kubernetes cluster from the Jenkins server with `kubectl`, you can follow the instructions in the [kubernetes-cluster-setup-playbook](https://github.com/mohammadrony/kubernetes-cluster-setup-playbook.git) repository to create a cluster with a master and worker node.

## Clone this repository

```bash
su - ansible
git clone https://github.com/mohammadrony/jenkins-docker-setup-playbook.git
```

## Check ansible hosts file

Check [hosts](./hosts) file for host-group and host-names with control plane and worker node host names. By default, it would work as follows

```bash
[admin]
admin-server
```

## Run Ansible playbook

```bash
ansible-playbook playbook.yml
```

## Start Jenkins service

```bash
jenkins_host="admin-server"
ssh ${jenkins_host} "sudo systemctl start jenkins"
ssh ${jenkins_host} "sudo systemctl status jenkins"
```

## Browse Jenkins dashboard from browser

- Visit <http://admin-server:8080> URL from local browser.
- Find the initial admin password from ansible control host's `/home/ansible/jenkins-admintoken.txt` file or Jenkins server host's `/var/lib/jenkins/secrets/initialAdminPassword` file.

Thank you.
