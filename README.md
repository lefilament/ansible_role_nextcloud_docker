docker_nextcloud
===============

This role deploys NextCloud in a Docker with Collabora Online and/or OnlyOffice
The main repo for this role is on [Le Filament GitLab](https://sources.le-filament.com/lefilament/ansible-roles/docker_nextcloud.git)

Requirements
------------

None

Role Variables
--------------

Variables from default directory :
* nextcloud_db_version: MariaDB version to be deployed (defaults to 10.5)
* nextcloud_version: NextCloud version to be deployed (defaults to 21)
* cloud_url: URL on which NextCloud will be listening
* cloud_db_root: Database root password
* cloud_db_pass: Database password
* cloud_admin_user: Owncloud Admin user
* cloud_admin_pass: Owncloud Admin password

* Collaborative edition
  * Collabora :
    * cloud_collabora: whether to deploy CollaboraOnline (defaults to false)
    * cloud_collabora_url: URL on which Collabora will be listening
    * cloud_collabora_admin_user: Collabora admin user
    * cloud_collabora_admin_pass: Collabora admin password
  * OnlyOffice
    * cloud_onlyoffice: whether to deploy CollaboraOnline (defaults to false)
    * cloud_onlyoffice_url: URL on which OnlyOffice will be listening

* Backups (for backups to be deployed, host needs to be in maintenance_contract group)
  * swift parameters for object storage instance where backups should be pushed weekly
  * cloud_backup_pass : Passphrase for encryption of backups

Dependencies
------------

This role requires the following Ansible collection :
* community.docker

This Docker role supposes that Traefik is deployed as an inverseproxy in front of the deployed Dockers.
The following role is used by Le Filament for deploying Traefik : docker_server (https://sources.le-filament.com/lefilament/ansible-roles/docker_server)

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: docker_nextcloud }
      vars:
         - { cloud_url: "cloud.example.org" }
         - { cloud_db_root: "veryUnsecureRootPassToBeModified" }
         - { cloud_db_pass: "veryUnsecurePassToBeModified" }
         - { cloud_admin_user: "admin" }
         - { cloud_admin_pass: "veryUnsecureAdminPassToBeModified" }

License
-------

AGPL-3

Author Information
------------------

Le Filament (https://le-filament.com)
