# Ansible playbook: rpi3\_wifi_ap

[![MIT licensed][mit-badge]][mit-link]

Ansible playbook for Access Point setup and configuration on Raspberry Pi.

Does the following:

 - Installs and configures `isc-dhcp-server`.
 - Installs and configures `hostapd`.
 - Installs and configures `iptables-persistent` (for now needs a reboot to load the iptables kernel modules).

It is assumed that the Raspberry Pi 3 or later (with built in broadcom wifi adapter) is used with additional Ralink usb wifi dongle.
It may also work with other RPI models, but just was not tested.

The one I use it with is Ralink USB WiFi RT5370:

```
 - 802.11 b/g/n 150Mbps;
 - 2.400-2.487 GHz channels 1-14;
 - Power: Voltage: 5V+5%, Amperage: 70mA Average)
```

Requirements
----

**NOTE:** Role requires Fact Gathering by ansible!

Debian-like linux distro installed

Playbook Variables
----

| Variable | Description | Default |
|----------|-------------|---------|
| `vault_rpi_wifi_ap_essid` | The Access Point ESSID | set your own in `vars/vault.yml` |
| `vault_rpi_wifi_ap_passphrase` | The Access Point WPA/WPA2 passphrase | set your own in `vars/vault.yml` |
| `vault_rpi_wifi_ap__rpi_network_wifi_APs` | The list of wifi networks to be configured on the system using the rpi_network role | set your own in `vars/vault.yml`, please check [`drew1kun.rpi_network/defaults/main.yaml`][net-aps-link] for reference |

Dependencies
----

 - [drew1kun.rpi_network][rpi_network-galaxy-link]
 - [drew1kun.rpi_wifi_ap][rpi_wifi_ap-galaxy-link]

Install via ansible-galaxy:

    ansible-galaxy install drew1kun.rpi_network \
                           drew1kun.rpi_wifi_ap

Playbook Usage Example
----
**ATTENTION!** variables are set in **vars/vault.yml**,
which is encrypted with [ansible-vault][ansible-vault-link].

Therefore, there are multiple options to run the play available:

### OPTION 0 [RECOMMENDED]:
add the following to **ansible.cfg**:

```
[defaults]
vault_password_file = ./vault.key
```

Modify the **vars/vault.yml** as you wish using:

```
ansible-vault edit vars/vault.yml
```

Then run play as follows:

```
ansible-playbook --user user -k rpi_wifi_ap_playbook.yml
```

### OPTION 1:
Before running the role decrypt the file `vars/vault.yml` with:

```
ansible-vault decrypt vars/main.yml --vault-password-file=vault.key
```

Then run play:

```
ansible-playbook -u user -k rpi_wifi_ap_playbook.yml
```

### OPTION 2:
Set environment variable:

```
export ANSIBLE_VAULT_PASSWORD_FILE=vault.key
```

Then run play:

```
ansible-playbook -u user -k rpi_wifi_ap_playbook.yml
```

### OPTION 3:

Just put the `.vault.key` to the playbook dir and run play:

```
ansible-playbook -u user \
		 -k rpi_wifi_ap_playbook.yml \
		 --vault-password-file=vault.key
```

License
----

[MIT][mit-link]

Author Information
----

Andrew Shagayev | [e-mail](mailto:drewshg@gmail.com)

[rpi_network-galaxy-link]: https://galaxy.ansible.com/drew1kun/rpi_network/
[rpi_wifi_ap-galaxy-link]: https://galaxy.ansible.com/drew1kun/rpi_wifi_ap/
[net-aps-link]: https://github.com/drew1kun/ansible-role-rpi_network/blob/master/defaults/main.yml
[ansible-vault-link]: https://docs.ansible.com/ansible/latest/user_guide/vault.html
[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[mit-link]: https://raw.githubusercontent.com/drew1kun/ansible-macos_setup/master/LICENSE
