# Mnist MLProject run automated with Ansible

## Pre-requisites

* Create a CSC virual machine via OpenStack web interface
    * instructions in  https://github.com/korolainenriikka/mlflow_test README: follow 'Initial setup instructions' 

On the control node:
* Install pip and python3-dev: `sudo apt update && sudo apt install -y python3-pip python3-dev`

* Install ansible: `sudo pip install ansible`

* Install OpenStack command line tools
    * `sudo pip install python-keystoneclient python-novaclient python-glanceclient python-neutronclient`
    * download OpenStack RC file from https://pouta.csc.fi/dashboard/project/api_access/
    * run `source <project-name>-openrc.sh` to add the env variables needed to use OpenStack CLI

* Configure ssh
    * to add a ssh keypair to control node: run  `nova keypair-add ansible-control-node > openstack-access-key`
    * start ssh agent and add private key to the agent with `eval $(ssh-agent) && shh-add [path-to-private-key]`

! adding env variables and starting ssh-agent has to be re-run every time a new control node terminal is launched.

## Run the project

* Clone this project on the control node

* Modify the `vars` file to contain your control node's floating IP address

* Create a virtual machine and set it up with MLflow requirements with `ansible-playbook create_vm.yml -i inventory.txt`

* Run the mnist project with `ansible-playbook run_mlproject.yml -i inventory.txt`

* Destroy the virtual machine and its environment after the run with `ansible-playbook cleanup.yml`

## Troubleshooting

* Auth plugin requires parameters which were not given: auth_url --> you are missing the env variables, download and source the RC file
