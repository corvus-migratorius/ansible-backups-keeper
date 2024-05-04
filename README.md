ansible-backups-keeper
=========

[![lint](https://github.com/corvus-migratorius/ansible-backups-keeper/actions/workflows/lint.yaml/badge.svg)](https://github.com/corvus-migratorius/ansible-backups-keeper/actions/workflows/lint.yaml)

Role for deploying the `backups-keeper` tool from a private repo

Requirements
------------

- Go
- ansible-git-clone (https://github.com/corvus-migratorius/ansible-git-clone)

Role Variables
--------------

`bkeeper_branch_clone_dest`: where to (temporarily) clone the `backups-keeper` repo

`bkeeper_repo`: SSH-style URL for the `backups-keeper` repo

`bkeeper_branch`: branch to be checked out after cloning (default: `master`)

`bkeeper_access_token`: `ansible-vault`-encrypted access token 

`bkeeper_timer_oncalendar`: systemd-style timer notation (default: `*:0/5`)

`bkeeper_ver`: version of the `backups-keeper` tool that needs to be installed

`bkeeper_spreadsheet_id`: Google spreadsheet ID

`bkeeper_read_range`: a cell range in A1 notation, or name of a sheet

`bkeeper_backup_manifests`: maps arbitrary backup "types" to filepaths, e.g.: `mirrored_daily: /root/backups-keeper/mirrored_daily.fofn`

Example Playbook
----------------

```yaml
    - role: ansible-backups-keeper
      tags: [ "bkeeper" ]
      bkeeper_gopath: "/usr/local/go/bin"
      bkeeper_branch_clone_dest: "/home/{{ansible_user}}/backups-keeper"
      bkeeper_repo: "git@github.com:corvus-migratorius/genlab-backups-keeper.git"
      bkeeper_branch: master
      bkeeper_ver: "0.2.1"
      bkeeper_timer_oncalendar: "*:0/1"
      bkeeper_access_token: <your-encrypted-token>
      bkeeper_spreadsheet_id: "1sLbq3SB5TEm1KW7srR5nwueotpVb36gG7yIydU1kC3A"
      bkeeper_read_range: "SheetName"
      bkeeper_manifests_dir: /var/local/backups-keeper
      bkeeper_backup_manifests:
        backup_manifests:
          mirrored_daily: "{{ bkeeper_manifests_dir }}/mirrored_daily.fofn"
          versioned_daily: "{{ bkeeper_manifests_dir }}/versioned_daily.fofn"
      bkeeper_creds_path: "{{ inventory_dir }}/secrets/bkeeper/credentials.json"

```

License
-------

BSD

Author Information
------------------

corvus-migratorius@proton.me
