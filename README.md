# Ansible_LearningLab_01
Part 1 of Ansible Learning

## Création du Lab

**Création d'un venv python pour installer Ansible**

```bash
python3 -m venv .VAnsible --prompt VAnsible
```

**Activation du venv**

```bash
source .VAnsible/bin/activate
```

**Désactivation du venv**

```bash
deactivate
```

**Installation d'Ansible**

```bash
python3 -m pip install --upgrade pip
pip3 install ansible
```

**Installation de Vagrant**

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
```

**Création d'un répertoire dédié aux playbooks**

```bash
mkdir playbooks
```

**Création d'une machine virtuelle via Vagrant**

```bash
vagrant init ubuntu/focal64
vagrant up
```

**Connexion en SSH à la VM**

```bash
vagrant ssh
```


**Création de l'inventaire ansible**

```bash
mkdir inventory
cat << EOF > inventory/vagrant.ini
[webservers]
testserver ansible_port=2222
[webservers:vars]
ansible_host=127.0.0.1
ansible_user=vagrant
ansible_private_key_file=.vagrant/machines/default/virtualbox/private_key
EOF
```

**Création d'un ansible.cfg pour qu'il utilise l'inventaire défini et pour qu'il ne fasse pas de host key checking ssh**

```bash
cat << EOF > ansible.cfg
[defaults]
inventory = inventory/vagrant.ini
host_key_checking = False
stdout_callback = yaml
callback_enabled = timer
EOF
```


### Commandes Ansible

**Ping du serveur défini dans l'inventaire depuis Ansible**

```bash
ansible testserver -m ping
```

**Obtenir l'uptime du serveur**

```bash
ansible testserver -a uptime
```

**Afficher les logs du serveur**

```bash
ansible testserver --become -a "tail /var/log/syslog"
```

**Installer un package sur le serveur**

```bash
ansible testserver --become -m package -a name=nginx
```

**Redémarrer un service**

```bash
ansible testserver -b -m service -a "name=nginx state=restarted"
```
