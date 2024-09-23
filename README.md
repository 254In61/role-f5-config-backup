# role-f5-config-backup
Role contains tasks that perform config backup of bigip/f5 devices

# Requirements
- Minimum ansible 2.11

# Role Variables

- Variables are fed into the role during the play.

- Varibles are set in group_vars/all.yml file

# How to use
- See example below

## Example vars file

! all-vars.yml
---
f5_git_repo_dir: f5

f5_access_port: 443

f5_ucs_file_name: "{{ inventory_hostname }}-ucs-backup.ucs"

f5_timeout: 600

## example playbook file
---
- name: Backup f5 Configuration

  hosts: appc_bigip_lb

  gather_facts: false

  connection: local

  vars:

    provider:

      server: "{{ ansible_host }}"

      user: "{{ f5_user }}"

      password: "{{ f5_password }}"

      validate_certs: false

      server_port: "{{ f5_access_port }}"

      timeout: 600

  pre_tasks:
    - name: Include specific project variables

      ansible.builtin.include_vars:

        dir: group_vars

      delegate_to: localhost

  tasks:

    - name: Set file operations facts

      ansible.builtin.set_fact:
        tmp_config_store: "{{ root_dir }}/{{ config_backup_file }}"

        git_repo_vendor_dir: "{{ f5_git_repo_dir }}"

        ucs_filename: "{{ f5_ucs_file_name }}"

      delegate_to: localhost

    - name: Import role-f5-config-backup role - config backup tasks

      ansible.builtin.import_role:

        name: role-f5-config-backup

      vars:

        f5_config: true

        f5_ucs: false
   
    - name: Import role-gitlab-config-backup-upload - upload running config backup

      ansible.builtin.import_role:

        name: role-gitlab-config-backup-upload

      vars:

        normal_file: true

        large_file: false

    - name: Import role-f5-config-backup role - ucs file backup tasks

      ansible.builtin.import_role:

        name: role-f5-config-backup

      vars:

        f5_config: false

        f5_ucs: true

    - name: Import role-gitlab-config-backup-upload - upload ucs file

      ansible.builtin.import_role:

        name: role-gitlab-config-backup-upload

      vars:

        normal_file: false

        large_file: true
    
  post_tasks:

    - name: Debug out results

      ansible.builtin.debug:

        msg:

          - "configuration backup tasks completed"
          
      delegate_to: localhost
...

# License
BSD

# Author Information
Name : Allan Maseghe

Company: Red Hat

Role: Consultant - Ansible
