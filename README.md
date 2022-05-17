# Ansible

Run an ad-hoc command (ping the hosts):

    $ ansible -i inventory.yml linux,openbsd -m ping -u patrick

Run a playbook:

    $ ansible-playbook -i inventory.yml -l linux,openbsd -u patrick playbook.yml
