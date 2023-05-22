# Working_On_Ansible
This repository contains the steps on setting up Ansible on AWS EC2 instance and how to spinup a Nginx server using Ansible.

# Install Ansible on AWS Ubuntu EC2 Instance

## Installations on Master Node:
1. Update the Master Node: sudo apt-get update
2. Install the "software-properties-common" packages: sudo apt install software-properties-common
3. Create the Ansible repository: sudo apt-add-repository ppa:ansible/ansible
4. Update "apt": sudo apt update
5. Install Ansible: sudo apt install ansible

## Installations on Host/Slave Nodes:
1. Update the Master Node: sudo apt-get update
2. Install Python3: sudo apt-get install python3

## Setup the connection between Master and Host/Slave nodes:
1. Connect from Master to Host/Slave node: ssh ubuntu@ipaddress #to connect from master to slave node (user@ipaddress)
2. Generate a SSH Key that will be used for the SSH authentication from the Master to the slave: ssh-keygen #to generate ssh rsa key to be used in th ssh connection

## Setting up Ansible on the Master node:
1. Open the hosts file: sudo nano /etc/ansible/hosts
2. At the end of the file add the following lines:
  [group_name]
  ans_slave ansible_ssh_host=ipaddress #a host/slave
3. Save the file
4. Check if ansible can access that host from the Master node: ansible -m ping group_name


# Deploy a playbook for building an Nginx server

## Install NGNIX
1. Create a yml file: sudo nano nginx-install.yml
2. Paste the code bellow:
    ---
    - name: set up ngnix webserver
      hosts: all
      become: true

      tasks:
      - name: ensure nginx is at the latest version
        apt:
          name: nginx
          state: latest
        vars:
          use_backend: apt

      - name: start nginx
        service:
          name: nginx
          state: started  # The service is active 
          enabled: yes  # if you want to also enable nginx

3. In the directory where you saved the nginx.install.yml file:
- To check if the tasks will be executed or there are errors run: - ansible-playbook -i /etc/ansible/hosts nginx-install.yml --check 
- To run the playbook and deploys the service on the hosts listed in the file /etc/ansible/hosts: - ansible-playbook -i /etc/ansible/hosts nginx-install.yml 

4. To stop the nginx service change the nginx-install.yml file to:
    ---
    - name: set up ngnix webserver
      hosts: all
      become: true

      tasks:
      - name: ensure nginx is at the latest version
        apt:
          name: nginx
          state: latest
        vars:
          use_backend: apt

      - name: start nginx
        service:
          name: nginx
          state: stopped # The service is stopped
          enabled: yes  # if you want to also enable nginx
