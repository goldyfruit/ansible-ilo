Ansible ilo
=========

This role will interact with HP iLO 4 via iLO RESTful API.

The iLO hostname and server name will be updated from the Ironic inventory

Supported features:
  - Change the power profile
  - Change the BIOS mode (EUFI/Legacy)
  - Update firmware via HTTP virtual media
  - Update iLO hostname
  - Update iLO server name
  - Reset the server
  - Collect MAC addresses
  - Enable/Disable PXE boot on interfaces

Requirements
------------

Ansible 2.1.x

Role Variables
--------------

```
# file: roles/ilo/defaults/main.yml
ilo_fqdn: eat.donuts.com
ilo_user: administrator
ilo_password: catchmeifyoucan
ilo_bios_mode: LegacyBios
ilo_bios_power_profile: MaxPerf
ilo_update_firmware: false
ilo_firmware_iso: http://10.10.10.10/871795_001_spp-2017.04.0-SPP2017040.2017_0420.14.iso
ilo_power_reset: false
ilo_mac: false
ilo_collect: true
ilo_network_adapter: 1
ilo_inventory_file: /opt/inventory.yml
ilo_macs_directory: /opt/macs
```

```
---
# file: roles/ilo/vars/Debian.yml
ilo_packages:
  - ipmitool
  - freeipmi-tools
  - sshpass
  - jq
```

```
# file: roles/ilo/vars/main.yml
ilo_ironic_options: "--os-auth-type token_endpoint --os-token fake --os-url http://127.0.0.1:6385"
ilo_ssh_opts: "-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null"
ilo_pip_packages:
  - netaddr
  - python-openstack
ilo_pxe_boot_enable:
  - NicBoot1
ilo_pxe_boot_disable:
  - NicBoot2
```

Dependencies
------------

None

Example Playbook
----------------

```
---
- name: iLO servers
  hosts: localhost

  vars:
    - ilo_bios_mode: Uefi
    - ilo_update_firmware: false

  roles:
    - ilo
```

Inventory file used by `ansible-ilo` role to get IPMI address via `ilo_inventory_file` variable.

```
---
inventory_nodes:
  - baremetal01:
      ipmi: 10.10.10.1
  - baremetal02:
      ipmi: 10.10.10.2
```

License
-------

BSD

Author Information
------------------

Gaëtan Trellu <gaetan.trellu@ormuco.com>
