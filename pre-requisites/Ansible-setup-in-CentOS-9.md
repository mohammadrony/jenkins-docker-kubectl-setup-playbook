# Setup Ansible control node and managed node in CentOS-9

## Node Naming convention

| Host name | Used terms |
|-----------|------------|
| Ansible host | control node |
| Jenkins server | managed node |
---------------------------------

## Configure IP address and host name in control node for ansible

```bash
echo '<ip-addr1> <host-name1>' >> /etc/hosts
echo '<ip-addr2> <host-name2>' >> /etc/hosts
```

## Configure IP address and host name in jenkins server

```bash
ssh root@$<jenkins-server> "echo '<ip-addr> <jenkins-server>' >> /etc/hosts"
```

## Install Ansible in control node

```bash
sudo dnf install -y epel-release
sudo dnf install -y ansible
```

## Create ansible user in control node

```bash
sudo useradd ansible
```

## Setup password for ansible user and login

Create new password

```bash
sudo passwd ansible
su - ansible
```

Or delete the password

```bash
sudo passwd -d ansible
su - ansible
```

## Generate ssh-key in control node as ansible user

```bash
ssh-keygen -f /home/ansible/.ssh/id_rsa
```

## Prepare ansible managed nodes

### Create ansible user in managed node

```bash
jenkins_server="<jenkins-server>"
ansible_home='/home/ansible'

ssh root@${jenkins_server} "useradd ansible"
ssh root@${jenkins_server} "mkdir -p ${ansible_home}/.ssh"
ssh root@${jenkins_server} "chmod 700 ${ansible_home}/.ssh"
ssh root@${jenkins_server} "chown -R ansible:ansible ${ansible_home}/.ssh"
```

### Copy id_rsa.pub key from control node to managed nodes .ssh/authorized_keys file

```bash
jenkins_server="<jenkins-server>"
ansible_home='/home/ansible'

scp ${ansible_home}/.ssh/id_rsa.pub root@${jenkins_server}:${ansible_home}/.ssh/id_rsa.pub
ssh root@${jenkins_server} "chown ansible:ansible ${ansible_home}/.ssh/id_rsa.pub"
ssh root@${jenkins_server} "tee < ${ansible_home}/.ssh/id_rsa.pub -a ${ansible_home}/.ssh/authorized_keys"
ssh root@${jenkins_server} "chown ansible:ansible ${ansible_home}/.ssh/authorized_keys"
ssh root@${jenkins_server} "chmod 600 ${ansible_home}/.ssh/authorized_keys"
```

### Add ansible to sudoers group for managed nodes

```bash
jenkins_server="<jenkins-server>"
ssh root@${jenkins_server} "echo 'ansible ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/ansible"
```

### Allow ansible user to execute command remotely from control node

```bash
jenkins_server="<jenkins-server>"

ssh root@${jenkins_server} "sed -i 's/#PubkeyAuthentication\syes/PubkeyAuthentication yes/' /etc/ssh/sshd_config"
ssh root@${jenkins_server} "sed -i 's/#AuthorizedKeysFile\s.ssh\/authorized_keys/AuthorizedKeysFile .ssh\/authorized_keys/' /etc/ssh/sshd_config"
ssh root@${jenkins_server} "systemctl restart sshd"
```

## Add host groups in control node for ansible user

```bash
sudo tee -a /etc/ansible/hosts << EOF
[<hostgroup>]
<host-name>
EOF
```

## Verify the connection is established between control node and managed node

```bash
ansible all -m ping
```

Thank you.
