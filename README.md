# Ansible role to deploy Nextcloud server docker and online document edition OnlyOffice and/or Collabora

[![](https://img.shields.io/badge/licence-AGPL--3-blue.svg)](http://www.gnu.org/licenses/agpl "License: AGPL-3")

This role allows you to deploy Nextcloud docker with associated MariaDB.

On top of deploying and starting the Nextcloud instance, the following functionality has been added:
- backup and restore based on duplicity docker from [Tecnativa](https://github.com/Tecnativa/docker-duplicity) which has been customized for our needs to backup on Public Object Storage (OpenShift) directory and running it every week using cron, keeping only the last 4 weeks backups (you can modify these parameters in templates/backups.yaml.j2)
- integration with OnlyOffice and/or Collabora CODE for NextCloud
- integration with LDAP network if necessary

Prior to running this role, you would need to have docker installed on your server and a traefik proxy (which is the purpose of [this role](https://github.com/lefilament/ansible_role_docker_server))

In order to use this role, you would need to define the following variables for your server (in hostvars for instance) - Only the names of the variables are provided below (not the values) for these used by this role to properly configure everything, you may copy this file directly in hostvars and set the variable although we could only encourage you to use an Ansible vault and refer vault variables from there:

```json
## Ansible configuration for connecting to remote host
# IP address of server
ansible_host: 
# User to be used on server (to which Ansible server public key has been provided)
ansible_user: 
# Encryped password (for elevating rights / sudo)
ansible_become_pass: 
# Server SSHD port
ansible_port: 


## Nextcloud configuration
# Nextcloud URL (only sub.domain without https:// in front)
cloud_url: 
# Nextcloud DB user/role
cloud_db_user: 
# Nextcloud DB password
cloud_db_pass: 
# Nextcloud MySQL root password
cloud_db_root: 
# Nextcloud Admin user
cloud_admin: 
# Nextcloud Admin password
cloud_admin_pass: 

## Swift Configuration
swift_cloud_username:
swift_cloud_password:
swift_cloud_authurl:
swift_cloud_authversion:
swift_cloud_tenantname:
swift_cloud_tenantid:
swift_cloud_regionname:

## OnlyOffice Configuration
# Document collaboration present for OnlyOffice ? (variable to be removed if not used)
cloud_onlyoffice: yes
# Document collaboration OnlyOffice URL (only sub.domain without https:// in front)
cloud_onlyoffice_url: 

## Collabora Configuration
# Document collaboration present for Collabora ? (variable to be removed if not used)
cloud_collabora: yes
# Document collaboration Collabora URL (only sub.domain without https:// in front)
cloud_collabora_url: 
cloud_collabora_admin_user:
cloud_collabora_admin_pass:
```
# Specific case for Collabora - configuration of Traefik Proxy
In order to get the Collabora CODE properly working when deployed on the same server as NextCloud and behind a proxy (as this is the case here), you would need to allow HTTPS connection between NextCloud and Collabora dockers (and thus through the proxy internally). In order to achieve this, you would need to add the following directives to your Traefik docker-compose configuration, after line 7 (under services / proxy / networks / shared) if you use the same Traefik configuration as we do (deployed from [this role](https://github.com/remi-filament/ansible_role_docker_server)):
```json
              aliases:
                - {{ cloud_url }}
		- {{ cloud_collabora_url }}
```

# Procedure to restore Nextcloud from backup

In order to restore Nextcloud database and files from backup, change directory to /home/docker/backups and run the following command:
`docker-compose -f backup-nextcloud.yaml run --rm backup_nextcloud sh -c "restore --force && mysql -h \$MYSQL_HOST -u \$MYSQL_USER -p\$MYSQL_PASSWORD \$MYSQL_DATABASE < \$SRC/mysql_db_\$MYSQL_DATABASE.sql"`



# Credits

## Contributors

* Remi Cazenave <remi-filament>


## Maintainer

[![](https://le-filament.com/img/logo-lefilament.png)](https://le-filament.com "Le Filament")

This role is maintained by Le Filament
