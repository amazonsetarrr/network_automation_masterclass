# network_automation_masterclass
repository resources for the class

## Cisco IOS XE Hostname Change Playbook with NetBox

This repository now follows a more standard Ansible layout. The playbook uses one role to retrieve device data from NetBox through a simple REST API call and a second role to render and push the hostname configuration to a Cisco IOS XE switch.

### Project Structure:
- `ansible.cfg`: Default inventory and role path configuration
- `playbook.yml`: Main playbook entry point
- `inventory.ini`: Inventory file with switch details
- `group_vars/all.yml`: NetBox connection variables shared by all hosts
- `roles/netbox_data/defaults/main.yml`: NetBox lookup default variables
- `roles/netbox_data/tasks/main.yml`: NetBox API lookup tasks
- `roles/config_push/tasks/main.yml`: Configuration rendering and push tasks
- `roles/config_push/templates/hostname_config.j2`: Hostname configuration template

### Usage:
1. Update `inventory.ini` with your switch's IP address, username, and password.
2. Update `group_vars/all.yml` with your NetBox URL and API token.
3. Make sure the NetBox device record exists. By default, the playbook looks up the device by `inventory_hostname`. If the NetBox name is different, set `netbox_device_name` for that host in `inventory.ini`.
4. Run the playbook: `ansible-playbook playbook.yml`

### Workflow:
1. `playbook.yml` targets the `switches` group and runs the `netbox_data` role first.
2. `netbox_data` queries NetBox at `/api/dcim/devices/?name=<device-name>`.
3. `netbox_data` reads the device `name` field from the JSON response and stores it as `desired_hostname`.
4. `config_push` renders `hostname <netbox-device-name>` from the template.
5. `config_push` pushes that configuration to the switch with `ios_config`.

### Example Tree:
```text
.
|-- ansible.cfg
|-- group_vars/
|   `-- all.yml
|-- inventory.ini
|-- playbook.yml
`-- roles/
    |-- config_push/
    |   |-- tasks/
    |   |   `-- main.yml
    |   `-- templates/
    |       `-- hostname_config.j2
    `-- netbox_data/
        |-- defaults/
        |   `-- main.yml
        `-- tasks/
            `-- main.yml
```

### Requirements:
- Ansible installed
- Network connectivity to the switch
- SSH enabled on the switch
- Network connectivity from the Ansible control node to NetBox
- A valid NetBox API token
