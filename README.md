# housekeeping

Performs various housekeeping tasks for Linux systems, such as pruning old journalctl logs, systemd coredumps, old cached packages, ansible async job logs, etc. Also supports custom housekeeping shell scripts on a per-host basis.

## Requirements

There are no requirements for this, it should work out of the box with Ansible.

## Role Variables

There are a few variables. See `defaults/main.yml` for values that can be tweaked on a per-host basis.

`housekeeping_user_scope_script` and `housekeeping_root_scope_script` can be defined as arbitrary shell scripts that will perform various cleanup tasks for each host. By default, these are empty strings, and should always be strings.

## Dependencies

<!-- A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles. -->

Requires`community.general.pacman` if an Arch Linux host is targeted.

## Usage

First, create a `requirements.yml` in your Ansible setup if you haven't already, here's an example:

```yaml
---
collections:
  - name: community.general
  - name: ansible.posix
  - name: community.crypto

roles:
  - name: charles-m-knox.housekeeping
    src: https://github.com/charles-m-knox/ansible-role-housekeeping.git
    scm: git
    version: main
```

Next, install it:

```bash
# include the -f flag to forceably re-install and get the latest (equivalent to
# upgrading)
ansible-galaxy role install -f -r requirements.yml
```

Now, in your `site.yml` (or whatever your playbook is named), use the role - note that root access is required for some of the steps:

```yaml
- name: housekeeping
  hosts: all
  roles:
    - charles-m-knox.housekeeping
  tags:
    - housekeeping
```

Some housekeeping tasks may not be desirable, in which case you may want to step through instead:

```bash
ansible-playbook site.yml --tags housekeeping --step
```

## License

MIT

## Author Information

<https://charlesmknox.com>
