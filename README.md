# Proposed Ansible Structure

## Purpose

The variable in our ansible repositories are currently a mess. I've been playing
around with the following structure as a replacement.

## Example Structure

```
├── development
│   ├── group_vars
│   │   └── all
│   │       ├── hosts.yml
│   │       ├── new_share.yml -> ../../../shared_vars/new_share.yml
│   │       ├── shared_test.yml -> ../../../shared_vars/test.yml
│   │       └── test.yml
│   └── inventory.ini
├── production
│   ├── group_vars
│   │   └── all
│   │       ├── hosts.yml
│   │       ├── new_share.yml -> ../../../shared_vars/new_share.yml
│   │       ├── shared_test.yml -> ../../../shared_vars/test.yml
│   │       └── test.yml
│   └── inventory.ini
├── shared_vars
│   ├── new_share.yml
│   └── test.yml
└── site.yml
└── vpass    # vault password - gitignore
```

## Changes

### Environments

All environments have their own directory that contains `group_vars` and an
`inventory.ini` file.

### Group vars

All group variables will be self contained within the environment. 

### Shared variables

Currently, the variables in `group_vars/all` should contain variables that are
shared between all environments. This is not the case.

All files in `shared_vars` will be symlinked into `<env>/group_vars/all`. This
will allow them to be picked up properly.

The current root level `group_vars` will be added to `.gitignore`

### Inventory Files

The inventory files can now be cleaned up to remove the environment specific
groups. By removing these groups, we can also more easily add dynamic hosts.

## Usage

```
ansible-playbook -i development/inventory.ini site.yml --vault-pass vpass
```
