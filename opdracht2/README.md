# Les 4 opdracht 2 IAC
Ruben Baas s1190828

# Ansible Lab - Webserver en Database Deployment

---

## Beschrijving van de opdracht
Dit project is onderdeel van een lab-opdracht waarin gebruik wordt gemaakt van Ansible om automatisch een webserver en een database server te configureren.

**Opdracht 1:**
- Deploy twee VM's (bijvoorbeeld via VirtualBox of VMware).
- Gebruik een Ansible-inventory om IP-adressen en hostnamen van beide VM's te beheren.
- Schrijf Ansible roles:
  - Voor de webserver (vm1): installeer `apache2`, `php`, en `php-mysql`.
  - Voor de database server (vm2): installeer `mysql-server`.
- De Ansible-inventory bevat groepsvariabelen en individuele host-variabelen.
- De `dbuser` en `dbpassword` worden gebruikt om verbinding te maken met de MySQL-database.
- Gebruik `handlers` en `variables` in de roles.
- Gebruik `group_vars` en `defaults`.

**Opdracht 2:**
- Onderzoek wat Ansible Galaxy is.
- Upload je gemaakte role naar Ansible Galaxy via een GitHub-repository.
- Maak een playbook dat je gepubliceerde role gebruikt.


---

## Structuur van de code

```text
.
├── ansible.cfg
├── inventory/
│   └── hosts.ini
├── group_vars/
│   ├── dbservers.yml
│   └── webservers.yml
├── roles/
│   ├── database/
│   │   ├── tasks/
│   │   ├── handlers/
│   │   ├── defaults/
│   │   └── meta/
│   └── webserver/
│       ├── tasks/
│       ├── handlers/
│       ├── defaults/
│       └── meta/
├── site.yml
└── requirements.yml (voor Galaxy)
```

### Belangrijke bestanden:
- `site.yml`: hoofd-playbook dat beide roles toepast.
- `inventory/hosts.ini`: bevat IP's en groepsindeling van de servers.
- `group_vars/`: hierin staan variabelen per groep, zoals databasewachtwoorden.
- `roles/webserver` en `roles/database`: bevatten de logica voor installatie van software.

---

## Benodigdheden

- Ansible (bijv. via `pip install ansible`)
- Twee draaiende Linux-VM's (bijv. Ubuntu Server)
- SSH-toegang tot beide VM's met een gebruikersaccount met sudo-rechten
- `ansible.cfg` moet correct zijn ingesteld voor je SSH-configuratie

---

## Uitvoeren van de playbooks

1. Pas het bestand `inventory/hosts.ini` aan met de juiste IP-adressen en hostnamen.
2. Pas indien nodig gebruikersnamen en wachtwoorden aan in `group_vars/`.
3. Voer het playbook uit:

```bash
ansible-playbook -i inventory/hosts.ini site.yml
```

---

## Publiceren naar Ansible Galaxy (Opdracht 2)

1. Maak een GitHub-repository met daarin je Ansible role.
2. Gebruik een correcte `meta/main.yml` in je role waarin je metadata zoals author en dependencies beschrijft.
3. Upload je role naar Ansible Galaxy via: https://galaxy.ansible.com/
4. Voeg je role toe aan `requirements.yml`:

```yaml
- src: <github_user>.<rolenaam>
  version: "1.0.0"
```

5. Installeer de role:

```bash
ansible-galaxy install -r requirements.yml
```

---

## Bronnen

Code uit opdracht 1 van les 4

AI prompt: op basis van de code en op basis van de opdracht : schrijf een readme.md waarin wordt beschreven wat de opdracht is, hoe de code werkt en hoe deze uitgevoerd moet worden.
https://chatgpt.com/share/6831d7e5-24dc-8007-afb2-9af4d7bac823

Ansible galaxy developer en user documenatie:
https://docs.ansible.com/ansible/latest/galaxy/dev_guide.html
https://docs.ansible.com/ansible/latest/galaxy/user_guide.html

