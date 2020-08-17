# crocodilehunter-rpi-ansible
Ansible Playbook to install Crocodile Hunter on Raspberry Pi 4

## Reqs
- Raspberry Pi 4 with accessible IP address, with ssh access enabled and the ssh fingerprint authorized on the host machine.
  - This playbook was tested with a fresh image of Raspbian (February 2020) flashed into the SD Card using [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/) and with a file named ssh (without extension) in the "boot" partition, with the ssh connection tested to authorize the fingerprint.
- Ansible installed in the host machine to run playbooks, in Ubuntu and similar distros it can be achieved with ```sudo apt install ansible```
- In this playbook, only the BladeRF radio unit is supported, other harware can be supported, however, this playbook won't install any drivers or other related software for it to work.

## How to use this playbook

1. First, locate the IP address of the connected Raspberry Pi to be used as a Crocodile Hunter sensor
2. Edit ```playbook.yml``` and edit/adjust the parameters to your setup. The following might be the most relevant values to change:
   -  wigle_name
   -  wigle_key
   -  ocid_key
   -  default_project_earfcns (in case you have this, the installation process will be more reliable)
3. run ```ansible-playbook -i <IP Address>, playbook.yml``` (if is just one IP address the comma is mandatory to make the command work)
4. Wait a little bit
5. Test and profit!

## Known issues

- Crocodile Hunter assumes that we have good GPS signal reception, if this is not the case, some processes can fail, especially the first run of the app, because it will try to get earfcns of the current location aquired with the GPS, so if it can't fix a location this process will last forever.
- In some cases, running ```sudo apt upgrade``` on the RPi make it unable to boot after all the install process.

# Todo

- When no gps reception on installation location run script to get arfcns with given coordinates and include them in config.ini.
- message at the beginning to check everything