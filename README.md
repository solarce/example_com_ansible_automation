example.com Automation
===========================

This repository contains automation for examples.com, including
Ansible playbooks and other utilities

Getting Started
---------------


In order to use the automation outlined below, you'll need to perform the following setup steps:


* [Install Ansible on the machine](http://docs.ansible.com/intro_installation.html#installing-the-control-machine) you'll be using to manage things.

*  Install a [git client](https://help.github.com/articles/set-up-git#platform-mac) and checkout the [example_automation]() repository into a directory, e.g.

` mkdir -p ~/code/example_automation/`

` git clone git@bitbucket.org:example/example_automation.git ~/code/example_automation/`

* Copy the SSH key into place so it can work with Ansible, with the following steps in Terminal.app

    * If you're using Windows, see http://www.bitvise.com/download-area and it's documentation for adding the SSH key

` cp ~/path/to/example_automation/ansible/roles/base/files/id_rsa_root* ~/.ssh/ `

` echo "Host do.example.com" >> ~/.ssh/config `

` echo "User root" >> ~/.ssh/config `

` echo "IdentityFile ~/.ssh/id_rsa_ " >> ~/.ssh/config `

Ansible Automation
------------------

* The `ansible` folder contains the ansible hosts file and the playbooks used to prepare a VPS to run example.com
* Review the README.md file inside it for further details on using the ansible automation

Using Ansible for Disaster Recovery
-----------------------------------

* Half of the reason for building the Ansible automation is to minimize downtime in the event of a disaster recovery scenario. These instructions assume there has been a failure of the existing DigitalOcean server such that it can't easily be returned to service and you're starting fresh.
* These steps assume you've completed the steps in the _Getting Started_ section and that you've still got the same SSH key uploaded to DigitalOcean.

1. Follow the steps in https://www.digitalocean.com/community/articles/how-to-create-your-first-digitalocean-droplet-virtual-server

 * Use 'example.com' as the server name
 * Use Ubuntu LTS as the distribution (12.04 was LTS at the time of this writing)
 * Don't select any pre-installed applications

2. Update DNS to point to the IP address of the new droplet on DreamHost. See http://wiki.dreamhost.com/Custom_DNS#Custom_A_Record

 * Do this for the following records:
 * example.com
 * do.example.com

3. Confirm you can SSH into the new host with your SSH key

`ssh do.example.com`

4. Do the initial Ansible run to prepare the server to run Wordpress and example.com

`cd ~/code/example_automation/`

`ansible-playbook ansible-playbook -i ./hosts site.yml`

* This will run and should produce output similar to https://gist.github.com/solarce/de955c418c441c349b4b

5. SSH into the server and install a fresh copy of Wordpress with the following commands

* http://codex.wordpress.org/Installing_WordPress#Famous_5-Minute_Install

`cd /var/www/example.com/`

`wget http://wordpress.org/latest.zip`

`unzip latest.zip`

6. Copy the example.com wp-config file to the server

`rsync -avP ~/code/example_automation/configs/wp-config.php do.example.com:/var/www/example.com/`

7. Restore from VaultPress using the steps in http://help.vaultpress.com/restore-to-a-new-site/. Make sure to select port 22 and the root username/password for the login information, so it'll do it over SSH/SFTP.

8. At this point the site should be working!
