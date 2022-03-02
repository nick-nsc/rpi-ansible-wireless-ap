# rpi-ansible-wireless-ap

## About

Configure a Raspberry Pi as a wireless access point with Ansible.

This project's configuration sets up a 5 GHz band (hw_mode=a, in hostapd.conf).  
Change this to hw_mode=g and select an allowed channel (something in the range of 1-12 should work) if you want a 2.4 GHz band for your access point.

Refer to [the official documentation](https://www.raspberrypi.com/documentation/computers/configuration.html#setting-up-a-bridged-wireless-access-point) for more information.

---

## Requirements

Before you run the playbook, you should do the following:

- [ ] set up rpi
  - [ ] set WLAN country code (in  raspi-config)
  - [ ] set new "pi"-user password
  - [ ] update packages
  - [ ] set a static IP for your RPI in /etc/dhcpcd.conf
  - [ ] security: change default ssh port
  - [ ] security: allow specific ssh users only (--> "pi"-user)
  - [ ] security: set up and enable ufw; allow new ssh port as well
- [ ] copy your public ssh key from the PC you are running Ansible on to the RPI (via ssh-copy-id)
- [ ] disable ssh via password --> enable ssh only via ssh keys

Also, have a look at the .example files and customize them as needed.  
For the playbook to run properly, you need to remove the .example suffix from the file names again.

---

## Run the playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Some Ansible ad-hoc commands

Get the hostname of each server from the rpi group in the inventory file:
```bash
ansible rpi -i inventory -m command -a "hostname"
OR
ansible rpi -i inventory -a "hostname"
```

Display all server information Ansible can get:
```bash
ansible rpi -i inventory -m setup
```

Install package ntp:
```bash
ansible rpi -i inventory --become -m dnf -a "name=ntp state=present"
OR
ansible rpi -i inventory -b -m dnf -a "name=ntp state=present"
```

Enable and start service ntpd:
```bash
ansible rpi -i inventory -b -m service -a "name=ntpd state=started enabled=yes"
```

Check the documentation for the service module:
```bash
ansible-doc service
```