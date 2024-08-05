# Learning Ansible

## Setup

Install Ansible (on Arch Linux):

    # pacman -S ansible
    $ ansible --version
    ansible [core 2.17.2]
    […]

Install Ansible (in a virtual Python environment):

    $ python3 -m venv env
    $ . env/bin/activate
    (env) $ pip3 install ansible
    (env) $ ansible --version
    ansible [core 2.17.2]
    […]

## Local VM

Make sure to have either libvirt or VirtualBox installed and working.

For libvirt, install the respective plugin first:

    $ vagrant plugin install vagrant-libvirt

Install Vagrant (on Arch Linux):

    # pacman -S vagrant
    $ vagrant --version
    Vagrant 2.4.1

Install VirtualBox (on Arch Linux):

    # pacman -S virtualbox virtualbox-host-modules-arch
    # usermod -a -G vboxusers patrick # replace "patrick" with your username

Setup a Debian Bookworm VM (or [any other](https://vagrantcloud.com/search)):

    $ vagrant init debian/bookworm64
    $ vagrant up

Connect to your VM via SSH, then close it again:

    $ vagrant ssh
    vagrant@bookrowm $ exit

Output the SSH configuration and open an SSH connection with that information:

    $ vagrant ssh-config
    $ ssh vagrant@127.0.0.1 -p 2222 -i .vagrant/machines/default/virtualbox/private_key

When finished testing, get rid of the Vagrant VM:

    $ vagrant destroy -f

## Inventory Setup

Create an inventory:

    $ mkdir inventory
    $ touch inventory/vagrant.ini

Add the following content to `inventory/vagrant.ini`:

```ini
[webservers]
testserver ansible_port=2222

[webservers:vars]
ansible_host=127.0.0.1
ansible_user=vagrant
ansible_private_key_file=.vagrant/machines/default/virtualbox/private_key
```

Test the connection by running Ansible's _ping_ module on it:

    $ ansible testserver -i inventory/vagrant.ini -m ping

Create a local Ansible config file (`ansible.cfg`) for convenience, which is
appropriate for a testing environment (with deactivated host key checking):

```ini
[defaults]
inventory=inventory/vagrant.ini
host_key_checking=False
stdout_callback=yaml
callback_enabled=timer
```

Test the connection using the default values from `ansible.cfg`:

    $ ansible testserver -m ping
    $ ansible testserver -m command -a uptime # -a is for "module arguments"
    $ ansible testserver -a uptime # the command module is the default module

Install a package (e.g. `htop`) using super user privileges (`-b`/`--become`):

    $ ansible testserver -b -a 'apt-get update' 
    $ ansible testserver -b -m package -a name=htop
