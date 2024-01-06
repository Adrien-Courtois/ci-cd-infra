# Pipeline CI/CD

### Vagrant

Lancer les VM
```
vagrant up
```

Un fichier `.env` est nécésaire à la racine du projet avec une variable `SSH_PUBKEY` représentant votre clé publique qui sera utilisé pour se connecter en SSH à la machine pour vous et ansible

### Ansible

Lancer le playbook ansible
```
ansible-playbook -i inventories/production/hosts -t TAG site.yml
```

Le `TAG` peut être :
- gitlab
- www

Ou rien si l'on souhaite l'exécuter sur tous les serveurs