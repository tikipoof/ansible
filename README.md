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

## To run ansible against Windows
On a Linux control machine:
```
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
pip install pywinrm
```

Prepare the target host
Download https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1
Run
```
powershell.exe -F .\Documents\ConfigureRemotingForAnsible.ps1
netsh advfirewall firewall add rule profile=any name="Allow WinRM HTTP" dir=in localport=5985 protocol=TCP action=allow
```

Run the playbook against a Windows host
```
ansible -i winhosts srv01.bpr.lab -m win_ping --ask-vault-pass -vvvv
```

## Use vault
```
mkdir host_vars
ansible-vault create host_vars/jumpny
```

Add the following lines
```
ansible_ssh_user: <username>
ansible_ssh_pass: <password>
ansible_ssh_port: 5985
ansible_connection: winrm
ansible_winrm_scheme: http
ansible_winrm_server_cert_validation: ignore
```

Run the playbook
```
ansible-playbook playbooks/role/all.yml -i hosts -l all --ask-vault-pass
```
