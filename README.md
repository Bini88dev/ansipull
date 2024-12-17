# Ansible Config...
Auto configuration for desktops, laptops, servers...

## Disclaimer
This repository contains a copy of the Ansible configuration that I use for desktops, laptops as well as servers.
Please don't directly use this against your own machines, as it is something I created for myself and may not translate to your use-case. It even configures OpenSSH, so if you run it you may get locked out.

## How does it work?
As mentioned above, it uses Ansible pull, so some familiarity with that is required. I use Ansible in "pull-mode" because it handles the dynamic nature of desktops, laptops and servers better. Afterall, they aren't always turned on...except server.

The folder structure:

**local.yml**: This is the Playbook that Ansible expects to find by default in pull-mode, think of it as an "index" of sorts that pulls other Playbooks in.

**ansible.cfg**: Configuration settings for Ansible goes here.

**group_vars/**: This directory is where I can place variables that will be applied on every system.

**host_vars/**: Each desktop/laptop/server gets a host_vars file in this folder, named after its hostname. Sets variables specific to that instance.

**hosts**: This is the inventory file. Even in pull-mode, an inventory file can be used. This is how Ansible knows what group bzw. role to put a machine in.

**playbooks**: Additional playbooks that I may want to run, or have triggered.

**roles/**: This directory contains my base, power, and server roles. Every host gets the base role. Then either 'power' or 'server', depending on what it is.

**roles/base**: This role is for every host, regardles of the type of device it is. This role contains things that are intended to be on every host, such as default configs, users, etc.

**roles/power**: After the base role runs on a host, this role runs only on hosts that are designated to be power. GUI-specific things, such as GUI apps (Firefox, etc), Flatpaks, wallpaper, etc.

**roles/server**: After the base role runs on a host, this role runs only on hosts designated as servers. Monitoring plugins, unattended-updates, server firewall rules, and other server-related things are configured here.

After it's run for the first time manually, this Ansible config creates its own Cronjob for itself on that machine so you never have to run it manually again going forward, and it will track all future commits and run them against all your machines as soon as you commit a change. You can find the playbook for Cron in the base role.
