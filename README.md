# Install Ansible

## Install ansible

```
apt-get update
apt-get install ansible git python-passlib sshpass
git clone https://github.com/tikipoof/ansible.git
cd ansible/
```


Modifier hosts pour faire apparaitre ansible dans les new
```
mkdir ~/.ssh
ssh-keyscan ansible > ~/.ssh/known_hosts
ansible-playbook playbooks/initialsetup/deb_initial.yml -i hosts -l new --ask-pass
rm -rf ansible
```

Depuis jumpny
```
ssh bpr@ansible 'mkdir -m 700 -p .ssh'
scp .ssh/authorized_keys bpr@ansible:.ssh/
scp .ssh/id_rsa* bpr@ansible:.ssh/

ssh ansible
git clone https://github.com/tikipoof/ansible.git
```

