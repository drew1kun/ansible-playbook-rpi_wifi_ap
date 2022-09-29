# ansible-playbook: rpi3\_wifi\_ap

[![MIT licensed][mit-badge]][mit-link]

Ansible playbook for Access Point setup and configuration on Raspberry Pi.

Does the following:

 - Installs and configures `isc-dhcp-server`.
 - Installs and configures `hostapd`.
 - Installs and configures `iptables-persistent` (for now needs a reboot to load the iptables kernel modules).

It is assumed that the raspberry pi 3(with built in broadcom wifi adapter) is used with additiional Ralink usb wifi dongle.

The one I use it with is Ralink USB WiFi RT5370:

```
 - 802.11 b/g/n 150Mbps;
 - 2.400-2.487 GHz channels 1-14;
 - Power: Voltage: 5V+5%, Amperage: 70mA Average)
```

Requirements
---

NOTE: Role requires Fact Gathering by ansible!

Debian-like linux distro installed

Playbook Variables
---

| Variable | Description | Default |
|----------|-------------|---------|
| `vault_wifi_ap_essid` | The Access Point ESSID | set your own in `vars/vault.yml` |
| `vault_wifi_ap_passphrase` | The Access Point WPA/WPA2 passphrase | set your own in `vars/vault.yml` |
| `vault_wifi_ap__rpi_network_wifi_APs` | The list of wifi networks to be configured on the system using the rpi_network role | set your own in `vars/vault.yml`, please check [`drew1kun.rpi_network/default/main.yaml`][net-aps-link] for reference |

Dependencies
------------

 - [drew1kun.rpi_network][rpi_network-galaxy-link]
 - [drew1kun.wifi_ap][wifi_ap-galaxy-link]

Install via ansible-galaxy:

    ansible-galaxy install drew1kun.rpi_network \
                           drew1kun.wifi_ap

Playbook Usage Example
---
**ATTENTION!** variables are set in **vars/vault.yml**,
which is encrypted with [ansible-vault][ansible-vault-link].

Therefore we have multiple options to run the play:

### OPTION 0:
Just put the `.vault.key` to the playbook dir and run play:

```
ansible-playbook -u user \
				 -k wifi_ap_playbook.yml \
				 --vault-password-file=.vault.key
```

### OPTION 1:
Before running the role decrypt the file *vars/main.yml* with:

```
ansible-vault decrypt vars/main.yml --vault-password-file=.vault.key
```

Then run play:

```
ansible-playbook -u user -k wifi_ap_playbook.yml
```

### OPTION 2:
Set environment variable:

```
export ANSIBLE_VAULT_PASSWORD_FILE=.vault.key
```

Then run play:

```
ansible-playbook -u user -k wifi_ap_playbook.yml
```

### OPTION 3 (PREFERRED):
add the following to **ansible.cfg**:

```
[defaults]
vault_password_file = .vault.key
```

Modify the **vars/vault.yml** as you wish using:

```
ansible-vault edit vars/vault.yml
```

Then run play as follows:

```
ansible-playbook --user user -k wifi_ap_playbook.yml
```



License
-------

[MIT][mit-link]

Author Information
------------------

Andrew Shagayev | [e-mail](mailto:drewshg@gmail.com)

[rpi_network-galaxy-link]: https://galaxy.ansible.com/drew1kun/rpi_network/
[wifi_ap-galaxy-link]: https://galaxy.ansible.com/drew1kun/wifi_ap/

[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[mit-link]: https://raw.githubusercontent.com/drew1kun/ansible-macos_setup/master/LICENSE
