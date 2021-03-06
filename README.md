# Vanderbilt_Cybersecurity
Things that I have learned and deployed while in Vanderbilt Cybersecurity class
![](Elk%20Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of any .yml file may be used to install only certain pieces of it, such as Filebeat.

## Playbooks
[My First Playbook.yml](https://github.com/natedanielson/Vanderbilt_Cybersecurity/blob/main/Playbooks/My%20First%20Playbook) - This playbook loads and enables docker. 

[Install Elk.yml](https://github.com/natedanielson/Vanderbilt_Cybersecurity/blob/main/Playbooks/Install-ELK) - This playbook installs elk using a docker container.

[Filebeat.yml](https://github.com/natedanielson/Vanderbilt_Cybersecurity/blob/main/Playbooks/Filebeat) - This playbook installs filebeat.

[Metricbeat.yml](https://github.com/natedanielson/Vanderbilt_Cybersecurity/blob/main/Playbooks/Metricbeat) - This playbook installs metricbeat.

This document contains the following details:

 - Description of the Topology
 - Access Policies
 - ELK Configuration
 - Beats in Use
 - Machines Being Monitored


## How to Use the Ansible Build


### Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

Load balancers are designed to take the punishment of the workload when systems try to access its servers.  It is really magical when using load balancers.  For example, the load balancer can create more server space automatically when it needs to.  IF it has different servers on the system then it can clone those to handle the traffic.  The advantage of having the load balancers on a jump box is to make sure that theweb servers don't go down and we can monitor them in a safe enviroment.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.

 - Filebeat: This program is used to log the files of web servers.
 - Metricbeat: This program is used to display the different metrics of an operating system on a server.

The configuration details of each machine may be found below.

| Name        | Function        | IP Address  | Operating System  |
| ------------|:---------------:| -----------:| ------------------|
| Jump-Box    | Provisioner     | 10.0.0.4    |Linux Ubuntu 18.04 |
| Web-1       | Docker/DVWA server | 10.0.0.5 |Linux Ubuntu 18.04 |
| Web-2       | Docker/DVWA server | 10.0.0.6 |Linux Ubuntu 18.04 |
| ELK-Server  | Elk-Stack      | 10.1.0.4     |Linux Ubuntu 18.04 |

## Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the Jump Box provisoner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

 - My personal IP Address

Machines within the network can only be accessed by SSH.

 - The only way to access the ELK server is by SSH via port 5601 on the JumpBox.

A summary of the access policies in place can be found in the table below.

| Name	| Publicly Accessible	| Allowed IP Address |
| ------------|:---------------:| ----------------|
|Jump-Box |	No	 | Personal IP Address |
|Web-1	| Yes via Load Balancer |	Load Balancer 137.117.41.225 / 10.0.0.4 JB |
|Web-2	| Yes via Load Balancer |	Load Balancer 137.117.41.225 / 10.0.0.4 JB |
|ELK-Server	 | No	| SSH 10.0.0.4 Via JB port 5601 |

## Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because by automating the process, we can run the playbook several times and build serveral virtual machines efficiently.

The playbook implements the following tasks:

 1. Install docker.io
 2. Install python3-pip
 3. Increse virtual memory
 4. Download and launch a docker elk container
 5. Enable service docker on boot



The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
![](Docker%20ps.PNG)


## Target Machines & Beats
This ELK server is configured to monitor the following machines:

 - Web-1 10.0.0.5
 - Web-2 10.0.0.6

We have installed the following Beats on these machines:

 - Filebeat
 - Metricbeat

These Beats allow us to collect the following information from each machine:

 - Filebeat is a system that is used to track the logs data and render it into readable charts and graphs.
 - Metricbeat records metrics from th operating system that is installed on the unit.
 - Both of these programs are lightweight virtual machines.


## Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

 - SSH into the control node and follow the steps below:

1. Copy the filebeat-config.yml and metricbeat-config.yml files to /etc/ansible/files.
2. Update the configuration files to include the IP address of your ELK machine.  The lines that need to be changed are 1106 (elasticsearch) and 1806 (kibana).
3. Run the playbook, and navigate to ELK-server-public-IP:5601/app/kibana to check that the installation worked as expected.

Which file is the playbook? 
 - [Install Elk.yml](https://github.com/natedanielson/Vanderbilt_Cybersecurity/blob/main/Playbooks/Install-ELK)

Where do you copy it?
 - /etc/ansible (on your ansible container)

Which file do you update to make Ansible run the playbook on a specific machine? 
 - /etc/ansible/hosts

How do I specify which machine to install the ELK server on versus which to install Filebeat on?
 - Edit the /etc/ansible/hosts file with the private IP's.  Example below:
 ![](etc-ansible-hosts.PNG)

Which URL do you navigate to in order to check that the ELK server is running?
 - http://public_ip_of_the_ELK-server;5601

## Ansible commands for Elk-server
1. ssh azureuser@jumpbox private IP - Enters the jump box
2. Sudo docker container list -a  - This will list all docker containers on the system.  The more you run your ansible playbook the more containers you wil have.
3. sudo docker start container_name  - This starts the ansible container.
4. sudo docker attach container_name  - This attaches the container and brings you to root of the container.
5. cd /etc/ansible  -  This brings you to the actual ansible container to install .yml files and also modify host files.
6. ansible-playbook insert_playbook_name_here.yml - This will run the playbook. ex. install_elk.yml, metricbeat.yml, etc.
7. Once everything is installed you can navigate to the elk_server_public_ip:5601/app/kibana to bring up the Kibana gui.
