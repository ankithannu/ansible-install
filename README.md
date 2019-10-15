# ansible-install


NOTE: THIS IMAGE IS TO BE USED FOR TEST AND LEARNIGN PURPOSES ONLY! NOT TO BE USED IN A PRODUCTION ENVIRONMENT!


The easy way to install ansible on ubuntu and running playbook using docker

Download the docker file "ankithannu/ansible:v1.1"
has a preinstalled os ubuntu:16.04 ,ansible installed and ssh and other required configurations done.
Please refer docker file ankithannu/ansible:v1.1 for details

# Spin up the container


docker run -itd --name ansible_master ankithannu/ansible:v1.1/bin/bash

docker run -itd --name ansible_node ankithannu/ansible:v1.1/bin/bash

# know the ip address of the running container node

docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ansible_node

Note the ip address of ansible_node '{{node-ip}}'

# Enter in the running master container

docker exec -it ansible_master sh

# Create a directory ansible

mkdir ansible
cd ansible

# Check the ssh service status and restart if required on master

service ssh status
service ssh restart

* sshd is running
# Check the ssh service status and restart if required on node by logging into another terminal

docker exec -it ansible_node sh
service ssh status
service ssh restart
* sshd is running

# From master
ping '{{node-ip}}'

# Generate the SSH key
 ssh-keygen
 Press enter 3 times as it ask ssh details.

# Copy the ssh key and connect to node via ssh and verify
ssh-copy-id root@'{{node-ip}}'

ssh root@'{{node-ip}}'

# Add entry in the host file in the end of the file
vi /etc/ansible/hosts

[machine]
'{{node-ip}}'

# Try to ping
ansible -m ping all

You will get the pong message means it is working perfectly and you can start working on ansible.


# Sample inventory file
inventory.txt
[machine]
node01 ansible_host='{{node-ip}}' ansible_connection=ssh ansible_user=root

sample playbook
---
- hosts: machine
  become: true
  tasks:
  - name: Install Package
    apt: name=vim,git state=latest

  # Run your first playbook
  ansible-playbook -i inventory.txt playbook.yml
  
  
  






