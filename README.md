# ansible-docker-compose

The purpose of this project is to be able to test Ansible roles locally and in Docker containers, with docker-compose.

The control node is Ubuntu and target "hosts" are centos-7, ubuntu-18 and ubuntu-20, with full init-systems and SSH access.  Basically, not the way you want to run containers but serves its purpose as a lightweight training environment.

### Getting Started
### Change into the root of your cloned repo and run the following commands
chmod 0600 ./env/ansible*
###
```
docker-compose up -d
```
###
cat env/ssh_host_config >> ~/.ssh/config

### Do this from within the repo root
ssh control 

### Running the example playbooks:
cd ansible/
###
ansible-playbook site.yml
###
ansible-playbook playbooks/stack_status.yml


### How to re-build Docker images:
docker-compose down
###
docker-compose build
###
docker-compose up -d
