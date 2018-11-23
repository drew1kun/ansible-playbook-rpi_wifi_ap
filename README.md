ansible-rpi3-wifi-access-point-playbook
=========

[![MIT licensed][mit-badge]][mit-link]

Ansible playbook for Raspberry Pi automated Access Point setup and configuration

Does the following:

 - Installs and configures `isc-dhcp-server`.
 - Installs and configures `hostapd`.
 - Installs and configures `iptables-persistent` (for now needs a reboot to load the iptables kernel modules).

It is assumed that the raspberry pi 3(with built in broadcom wifi adapter) is used with additiional Ralink usb wifi dongle.

The one I use it with is Ralink USB WiFi RT5370:
 - 802.11 b/g/n 150Mbps;
 - 2.400-2.487 GHz channels 1-14;
 - Power: Voltage: 5V+5%, Amperage: 70mA Average)

Requirements
------------

NOTE: Role requires Fact Gathering by ansible!

Debian-like linux distro installed

Playbook Variables
------------------

| Variable | Description | Default |
|----------|-------------|---------|
| **vault_wifi_ap_essid** | The Access Point ESSID | set your own in `vars/vault.yml` |
| **vault_wifi_ap_passphrase** | The Access Point WPA/WPA2 passphrase | set your own in `vars/vault.yml` |

Dependencies
------------

 - [drew-kun.rpi3_network][rpi3_network-galaxy-link]
 - [drew-kun.wifi_ap][wifi_ap-galaxy-link]

Install via ansible-galaxy:

    ansible-galaxy install drew-kun.rpi3_network \
                           drew-kun.wifi_ap

Example Using Playbook
----------------------

    ansible-playbook --user user -k wifi_ap_playbook.yml --vault-password-file=.vault.key

License
-------

[MIT][mit-link]

Author Information
------------------

Andrew Shagayev | [e-mail](mailto:drewshg@gmail.com)

[rpi3_network-galaxy-link]: https://galaxy.ansible.com/drew-kun/rpi3_network/
[wifi_ap-galaxy-link]: https://galaxy.ansible.com/drew-kun/wifi_ap/

[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[mit-link]: https://raw.githubusercontent.com/drew-kun/ansible-macos_setup/master/LICENSE
