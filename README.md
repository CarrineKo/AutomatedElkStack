## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/1xDhP-pkR8BKyU0tHd9b5kYslkwVATuMt/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  -https://github.com/CarrineKo/AutomatedElkStack/blob/Ansible/ansible.cfg.txt

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting DDoS attacks to the network.

Load Balancers protect Network Security by preventing DDos attacks. The advantages of a jumpbox include the extensive software library, automated backups as well as restricted access to the corporate network. It serves as a single point of remote access.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to data and system logs.

Filebeat monitors the log files, collects events and forwards them to a specified location.

Metricbeat monitors the servers and logs metrics and statistics.

The configuration details of each machine may be found below. Each VM is established on a Linux based Ubuntu OS.

|   Name   | Function | IP Address | Operating System |
|:--------:|:--------:|:----------:|:----------------:|
| Jump Box |  Gateway |  10.0.0.4  |       Linux      |
|   Web-1  |  Server  |  10.0.0.5  |       Linux      |
|   Web-2  |  Server  |  10.0.0.6  |       Linux      |
|    Elk   |  Server  |  10.1.0.4  |       Linux      |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
70.44.56.93

Machines within the network can only be accessed by Jump Box. Jump Box is the only machine that has access to ELK VM. Jump Box has a public IP of 20.127.109.75 and Private IP of 10.0.0.4. 
ELK VM has a private IP of 10.1.0.4.

A summary of the access policies in place can be found in the table below.

|   Name   | Publicly Accessible | Allowed IP Address |
|:--------:|:-------------------:|:------------------:|
| Jump Box |         Yes         |     70.44.56.93    |
|   Web-1  |          No         |      10.0.0.4      |
|   Web-2  |          No         |      10.0.0.4      |
|    Elk   |          No         |      10.0.0.4      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
-The main advantage of using ansible to automate configuration is the flexibility and simplicity. It's easy to setup and use. 

The playbook implements the following tasks:
- Install docker.io 
	-this is the deb package used by Ubuntu to be able to install the docker containers
	-state=present simply means to install the package
- Install pip3
	-this is the package manager for python
- Install Docker
	-docker is the conatiner 
- Download and Launch DVWA
	-Damn Vulnerable Web Application is software that intentionally includes vulnerabilities for training and education
- Enable Docker Service
	- system comman that enables the docker to start on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
	-10.0.0.5 
	-10.0.0.6

We have installed the following Beats on these machines:
	-Filebeat
	-Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors log files and records log events to ship them to the ELK stack for analysis. and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
	- ex. Apache Access Logs - /var/log/apache2 

- Metricbeat monitors and ships various system and service metrics such as: 
	- CPU, memory, and network

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat configuration file to /etc/ansible/filebeat-config.yml.
- Update the filebeat-config.yml file to include the IP address of the ELK VM.

		output.elasticsearch:
		hosts: ["10.1.0.4:9200"]
		username: "elastic"
		password: "changeme"

		...

		setup.kibana:
		host: "10.1.0.4:5601"

- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

		http://[your.VM.IP]:5601/app/kibana. 

##CHECKLIST
- filebeat-playbook.yml is the playbook and it gets copied to /etc/ansible/roles/filebeat-playbook.yml
- Update the ansible hosts file to specify which machine to run playbook on. 
	- ELK server gets installed using the [elk] group in the hosts file:
		
		- name: Config elk VM with Docker
  		  hosts: elk
  		  remote_user: azadmin
  		  become: true
  		  tasks:

	- Filebeat gets installed by using the [FBServer] group in the hosts file:
		
		- name: installing and launching filebeat
  		  hosts: FBServers
  		  become: true
  		  tasks:
		
- In order to check that the ELK server is running, navigate to:
	- http://[your.VM.IP]:5601/app/kibana


_##**Bonus Commands**

- Creating a NEW containers (FIRST TIME ONLY)
	
	- sudo docker run -ti cyberxsecurity/ansible:latest	

- List existing containers

	- sudo docker cotainer list -a

- Start existing container

	- sudo docker start [container_name]

- Attach existing container (use after start)

	- sudo docker attach [container_name]

- Retrieve and copy the filebeat configuration file:
	
	- curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat >> /etc/ansible/filebeat-config.yml
	
- Edit hosts file:
	
	- nano /etc/ansible/hosts	

- Edit configuration files:
		
	- nano /etc/ansible/ansible.cfg
	- nano /etc/ansible/filebeat-config.yml
	- nano /etc/ansible/metricbeat-cofig.yml

- Run the playbooks:
	
	- ansible-playbook /etc/ansible/roles/filebeat-playbook.yml
	- ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml
	- ansible-playbook /etc/ansible/install-elk.yml
	- ansible-playbook /etc/ansible/pentest.yml
