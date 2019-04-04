# Ansible-catapult
using ansible to deploy catapult private chain in multiple server.

# Ansible structure
```
+-- inventories         // all target host configuration 
|   +-- group_vars
|     +-- all.yml
|   +-- host-nodes.yml
+-- roles               // software dependencies 
|   +-- common
|   +-- nickjj.docker
+-- ansible.cfg         // Ansible local directory configuration
+-- deploy-config.yml   // upload all config file to target host
+-- initial-remote-host.yml  // install software requirement in target host
+-- patch-peer-info.yml      // patch all peer info to all target node
+-- server-test.yml          // basic testing make sure all target host is reachable
```

# Let start
By using AWS EC2 service to launch 3 in instance VM (t2.micro) with volume 40Gb.

# Install Ansible in your machine

please Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#)

# Config Setup target host

1. open `inventories/host-nodes.yml` and update your server ip from your EC2 services.
2. execute ansible-playbook to test on all target host connection.

`ansible-playbook -i inventories/host-nodes.yml -b server-test.yml --private-key=aws-server.pem`

# Install software requirement
execute ansible-playbook to install software requirement to run a catapult private chain.

`ansible-playbook -i inventories/host-nodes.yml -b initial-remote-host.yml --private-key=aws-aws-server.pem`

# Upload config to target host (API/Peer)
1. upload all config and file from local to target host, API node and Peer node are using different configuration.

`ansible-playbook -i inventories/host-nodes.yml -b deploy-config.yml --private-key=aws-aws-server.pem`

2. once targed host have the config, manually configure  (still working in automation)

# Patch Peers-api.json & Peers-p2p.json to all target host
1. peers-api & peers-p2p is use for maintain peer list in private chain.
2. all node should have same copy of the peer list infomation. by using `ansible-playbook -i inventories/host-nodes.yml -b patch-peer-info.yml --private-key=aws-aws-server.pem`
we can maintain all node keep the same copy of the peer list.
