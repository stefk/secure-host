# Secure host

Ansible role for minimal securisation of a newly provisionned
Debian/Ubuntu server, typically as delivered by vendors such
as Digital Ocean or OVH. This role will:

- create a non-root user with SSH key and sudo privileges
- disable password authentication and root login
- install fail2ban
- setup an UFW firewall rejecting all incoming traffic except for SSH

## Roles Variables

All the variables are related to the user that will be created:

- `user_name`: username for the new account
- `user_pass`: encrypted password
  (doc [here](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module))
- `user_key`: SSH public key

## Example Playbook

```
- hosts: all
  roles:
    - role: secure-host
      vars:
        user_name: johndoe
        user_pass: $6$3FHGDgs$sPZqQJqilRC7JV2dlXAtG5rOzgeAnYuut24LftgyTY/ou/4skFMvqtDhD9jjYmRW.ry7A4BYX3ExorcrnSH0
        user_key: "{{ lookup('file', '/home/john/.ssh/id_rsa.pub') }}"
```

Usage example:

```
ansible-playbook -i hosts -l server1 -u root -k -K secure.yml
```

