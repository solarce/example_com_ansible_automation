Ansible Automation
==================

The Ansible automation is geared towards two things

1. Maintain the state of the server externally, and being able to change that state by updating/adding roles.
2. Preparing a new server in case of Disaster Recovery

* This README briefly describes the contents of the folder and their purposes. Further details of specific roles or files can be found as inline comments in each file.

site.yml
--------

* This file determines what ansible will _do_ to hosts you manage with it

hosts
-----

* List of "sites", really a list of the servers

Variables
---------

* The `group_vars` folder contains the variables that various roles will use to set things like the docroot folder, domain name, database logins, etc.


Roles
-----

* _apache:_ This role installs the apache and php packages needed, creates the directory for wordpress, and puts config files in place for the site.

* _base:_ This role handles some base packages and managed __/etc/hosts__.

* _memcached:_ This role installs the memcached service and creates a config file for it.

* _mysql:_ This role installs MySQL and creates the needed database and users.

* _serverdensity-agent:_ This role is a third party repository from https://github.com/daviddoran/ansible-patterns/tree/master/server-density-agent for installing the ServerDensity agent and configuring it.
