# ansible-role-create-droplet
Ansible role for creating DigitalOcean droplets,
with an emphasis on sane defaults for parameters.

## Overview
The motivation for this role was frustration around the fact that
Ansible doesn't provide a method for declaring default variables
to modules. Without default variables, every module invocation
is unnecessarily verbose:

```
- name: create digitalocean droplet
  digital_ocean:
    command: droplet
    state: present
    name: testbox
    unique_name: true

    size_id: 512mb
    region_id: sfo1
    # Debian 8 64-bit has slug "debian-8-x64" and id "14169880"
    image_id: "14169880"
    private_networking: true

    api_token: "{{ digitalocean_api_token }}"
    ssh_key_ids: "{{ digitalocean_ssh_key_ids | join(',') }}"
```

The only new variable in the above block is `name: testbox`.
All other variables should reference defaults. Using a role
as a surrogate module makes referencing default variables possible.

## Usage

```
- name: create digitalocean droplet
  hosts: localhost
  roles:
    - { role: create-droplet,
        droplet_name: testbox,
      }
```
Ah, that's better!

## License
MIT
