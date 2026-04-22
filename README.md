# network_automation_masterclass
repository resources for the class

## Basic Cisco IOS XE Hostname Change Playbook

This repository contains a basic Ansible playbook to change the hostname of a Cisco IOS XE switch.

### Files:
- `playbook.yml`: The Ansible playbook
- `inventory.ini`: Inventory file with switch details
- `group_vars/switches.yml`: Variables for the switches group

### Usage:
1. Update `inventory.ini` with your switch's IP address, username, and password.
2. Update `group_vars/switches.yml` with the desired new hostname.
3. Run the playbook: `ansible-playbook -i inventory.ini playbook.yml`

### Requirements:
- Ansible installed
- Network connectivity to the switch
- SSH enabled on the switch
