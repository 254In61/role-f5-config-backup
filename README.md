# role-cisco-config-backup
Ansible role to backup cisco devices configs.

# How to use
Step 1: Install the role in your environment.
   - You could have roles/requirements.yml if running on AAP.
     Project synch/source version control AAP job, which runs before the template job, will install the roles on the execution environment.
   - Or simple install on your environment.

Step 2: Define your variables

Step 3: Call the role from your playbook.

# Example use

## roles consumption
- See example-requirements.yml

## example playbook
- See example-playbook.yml

## Notes
- Role extracts the running configuration and dumps them on a file in the local disk.
- Tasks have to be included to send this file somewhere e.g git repo or to an artifacts store.
