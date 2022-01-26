# crocodilehunter-rpi-ansible
Ansible Playbook to install Crocodile Hunter on Raspberry Pi 4

## Reqs
- Raspberry Pi 4 with an available IP address, with ssh access enabled and the ssh fingerprint authorized on the host machine.
  - This playbook was tested with SD cards flashed with [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/) using a fresh image Raspberry Pi OS (Buster version, see the Known issues section). After flashing, adding a file named ```ssh``` (without extension) in the "boot" partition, with the ssh connection tested to authorize the fingerprint.
- Ansible installed in the host machine to run playbooks, in Ubuntu and similar distros it can be achieved with ```sudo apt install ansible```
- In this playbook, only the BladeRF x40 radio unit is supported, Crocodile Hunter supports other radio units, however, this playbook won't install any drivers or other related software for those.

## How to use this playbook

1. First, locate the IP address of the connected Raspberry Pi to be used as a Crocodile Hunter sensor.
2. Edit ```playbook.yml``` and edit/adjust the parameters to your setup. The following might be the most relevant values to change:
   -  wigle_name
   -  wigle_key
   -  ocid_key
   -  default_project_earfcns (in case you have this, the installation process will be more reliable)
   -  expected_mccs and expected_mncs if you want to include this analysis and setting check_geographic_codes to true
   -  api_host, api_port, and api_contact if applicable.
3. run ```ansible-playbook -i <IP Address>, playbook.yml``` (if we have just one IP address the comma is mandatory)
4. Wait a little bit
5. Test and profit!
6. If applicable, we recommend following manually the proposed steps to generate an API key for the configured server using ```export CH_PROJ=chunter; python3 api_client.py signup```, and adding the commands ```export CH_PROJ=chunter; python3 api_client.py add_towers``` to a recurent cronjob (the frequency might be dictated by the specific need of the operation and/or the associated threat model).

## Known issues

- Crocodile Hunter assumes that we have a good GPS signal reception. If this is not the case, some processes can fail, especially the first run of the app, because it will try to get earfcns of the current location acquired with the GPS, so if it can't fix a location, this process will last forever.
- In some cases, running ```sudo apt upgrade``` on the RPi makes it unable to be accessible through ssh after the installation process. It is advisable to run ```sudo raspi-config``` after the upgrade and (re)enable ssh before shutting down or rebooting the Raspberry Pi.
- We experienced issues excuting the playbook on Raspberry Pi OS Bullseye, so while we troubleshoot, we strongly recommend installing Raspberry Pi OS Buster.
- We also experienced issues using the latest versions of BladeRF Firmware and FPGA for the x40 model, we added to ```playbook.yml``` the possibility of controlling the respective versions.

# Todo

- TBD