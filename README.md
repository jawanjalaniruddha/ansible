# Ansible set-up

Purpose of this document is to get up and running with Ansible with fixed directory structure. We will refer standard practices and initialization approach with Ansible to get new comers upto the speed. 

The directiry structure that we are going to follow will be as below. 

## Ansible Directory Structure
```
mactest:ansible aniruddhajawanjal$ pwd
/Users/aniruddhajawanjal/Devops/Tools/ansible
mactest:ansible aniruddhajawanjal$ tree
.
├── README.md
├── Vagrantfile
├── ansible.cfg
├── inventory
│   ├── production
│   ├── staging
│   └── vagrant
├── playbook.yml
└── roles
    ├── common
    │   ├── ssh-devops
    │   │   ├── README.md
    │   └── sys-manager
    │       ├── README.md
    ├── custom
    └── galaxy

20 directories, 25 files
```

Where,

* README.md: This file
* Vagrantfile: Vagrantfile to test Ansible roles with Vagrant
* Inventory: This directory will contain the inventory files for each environment with server details where Ansible will run. 
* playbook.yml: This will be Ansible playbook. It can be environment specific such as staging.yml, production.yml or functionality specific such as webserver.yml, dbserver.yml. It can be decided depending on the requirement. 
* Roles: It contains all the roles required for Project. Each directory will have different goal.
		common: This directory will have roles which will be used for all servers  i.e. common roles
		custom: custom directory will contain specific roles created for project requirement. 
		galaxy: Roles downloaded from Ansible Galaxy.

--------------------------------------------------------------

## Ansible Config file

`ansible.cfg` file is INI format file which contains configuration options for Ansible run. We prefer to keep it in the directory where project code is kept. Ansible will look for ansible.cfg file in following order.

* `ANSIBLE_CONFIG` (environment variable if set)
* `ansible.cfg` (in the current directory)
* `~/.ansible.cfg` (in the home directory)
* `/etc/ansible/ansible.cfg`

Ansible will process the above list and use the first file found, all others are ignored. 

--------------------------------------------------------------


## Usefull Commands 

### Checking server reachability
```
ansible all -m ping
```

### Creating a new role
```
ansible-galaxy init test-role-1
```

### To install Ansible Galaxy role: 
```
ansible-galaxy install --roles-path . geerlingguy.apache
```

### Playbook dry run
```
ansible-playbook foo.yml --check --diff
```

### Limiting task with tags
```
ansible-playbook playbooks/PLAYBOOK_NAME.yml --tags 'install'
```

### Skip roles with Tags matching
```
ansible-playbook playbooks/PLAYBOOK_NAME.yml --skip-tags 'sudoers'
```

### Checking Synatax
```
ansible-playbook playbooks/PLAYBOOK_NAME.yml --syntax-check
```

--------------------------------------------------------------

## Ansible with Vagrant 

https://docs.ansible.com/ansible/latest/scenario_guides/guide_vagrant.html


We will be using vagrant provisioner. For the first time, you will need to do `vagrant up` to get vagrant started with Vagrantfile in place. 

One issue that I faced was, vagrant machine didn't have python installed which is required for Ansible. So, I had to do `vagrant ssh`, install python and do `vagrant provision` again. Every time we make any change to playbook.yml, we will need to run `vagrant provision`.

--------------------------------------------------------------

## Manipulating secrets in Ansible

We can either use Ansible Vault or AWS SSM or Hashicorp Vault depending on the project requirement. Better approach would be to use Ansible Vault as this is piece of configuration management solution. If we want to avoid any dependency on some specific tool, it would make more sense to use third party tool like Hashicorp Vault to store the secrets, credentials etc. 

--------------------------------------------------------------

### Ansible with Windows







