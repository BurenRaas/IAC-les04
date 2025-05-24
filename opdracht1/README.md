# IAC les 04 – Opdracht 1
Ruben Baas s1190828

## Doel van de opdracht

Deze opdracht automatiseert de installatie en configuratie van twee virtuele machines (VM’s):

- Webserver: installeert Apache, PHP en PHP-MySQL.
- Databaseserver: installeert MySQL en configureert een gebruiker.

De infrastructuur wordt uitgerold met **Terraform**. De configuratie van de servers gebeurt met **Ansible**. De IP-adressen, hostnamen en SSH-instellingen worden automatisch opgenomen in het bestand `inventory.ini`.

---

## Infrastructuur aanmaken

Voer onderstaande commando's uit:

```bash
terraform init
terraform apply
```

Tijdens `terraform apply` wordt het volgende Ansible-inventorybestand gegenereerd via een `null_resource`:

```hcl
resource "null_resource" "generate_ansible_inventory" {
  provisioner "local-exec" {
    command = <<EOT
echo "[webserver]" > inventory.ini
%{ for ip in esxi_guest.webserver[*].ip_address ~}
echo "${ip} ansible_user=student ansible_ssh_private_key_file=~/.ssh/iac" >> inventory.ini
%{ endfor ~}

echo "" >> inventory.ini
echo "[databaseserver]" >> inventory.ini
echo "${esxi_guest.databaseserver.ip_address} ansible_user=student ansible_ssh_private_key_file=~/.ssh/iac" >> inventory.ini
EOT
  }
}
```

---

## Stap 2: Ansible Playbook uitvoeren

Voer het volgende commando uit:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

### Wat gebeurt er?

- De webservergroep:
  - Installeert `apache2`, `php`, `php-mysql`.
  - Plaatst een webpagina in `/var/www/html/index.html`.
  - Herstart Apache via een handler.

- De databaseservergroep:
  - Installeert `mysql-server` en `python3-pymysql`.
  - Maakt gebruiker `dbuser` aan met wachtwoord `dbpassword`.
  - Start en activeert de MySQL-service.

---

## Bronnen

Ansible install package
https://www.cyberciti.biz/faq/ansible-apt-update-all-packages-on-ubuntu-debian-linux/

Lokaal bestand kopieeren
https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html

Ansible mysql user:
https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_user_module.html

Ansible start service
https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html

AI prompt: foutmelding bij het aanmaken van een mysql_user tijdens Ansible playbook
https://chatgpt.com/share/68302b8d-e550-8007-9ec3-d425194f8e80

Ai prompt: Herschrijf deze README op basis van de meegeleverde code.
https://chatgpt.com/share/6830b813-11e4-8007-aaf7-39c45bda2647 

Repo link:
https://github.com/BurenRaas/IAC-les04