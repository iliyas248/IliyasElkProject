# IliyasElkProject
 
 ## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

https://github.com/iliyas248/IliyasElkProject/issues/2

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

![filebeat_playbook_yml](https://user-images.githubusercontent.com/83253408/130723627-99b1207d-9c03-4806-baeb-4442cdc1e5cd.png)

1. elk-playbook.yml: install and launch sebp/elk container on ELK server
2. filebeat-configuration.yml: the pre-defined configuration file of Filebeat which has updated Kibana and Elasticsearch server information
3. filebeat-playbook.yml: ansible playbook file to be run on Ansible container on Jumpbox to install Filebeat on both Web1 and Web2 servers
4. metricbeat-configuration.yml: the pre-defined configuration file of Metricbeat which has updated Kibana and Elasticsearch server information
5. metricbeat-playbook.yml: ansible playbook file to be run on Ansible container on Jumpbox to install Metricbeat on both Web1 and Web2 servers

### This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

## Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the filesystem and system performance. The ELK stack is in a separate region and virtual network. This helps separate operational services with production services.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_




| Name     	| Function                                     	| IP Address 	| Operating System     	|
|----------	|----------------------------------------------	|------------	|----------------------	|
| Jump Box 	| Gateway                                      	| 10.0.0.4   	| Linux (ubuntu 20.04) 	|
| Web-1    	| Host a Container DVWA                        	| 10.0.0.5   	| Linux (ubuntu 20.04) 	|
| Web-2    	| Host a Container DVWA                        	| 10.0.0.8   	| Linux (ubuntu 20.04) 	|
| ELK      	| Monitoring & log aggregation for Webs 1 & 2  	| 10.1.0.5   	| Linux (ubuntu 20.04) 	|
|          	|                                              	|            	|                      	|




### Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
73.136.222.109 (My Local Home IP)

Machines within the network can only be accessed by Jump Box or ELK Machine via SSH
Jump box ( 10.0.0.10)  ELK Machine  ( 10.2.0.5)

A summary of the access policies in place can be found in the table below.
| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 73.136.222.109       |
| Web-1    | No                  | Local Vnet           |
| Web-2    | No                  | Local Vnet           |
| ELK      | Yes                 | 73.136.222.109       |

### Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- _TODO: What is the main advantage of automating configuration with Ansible?

The main advantage of the installation process is that multiple servers can easily and quickly be deployed without having to physically touch each server. Going to every machine and configuring individually can be time consuming and tedious
_
The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
| Name     | Function           | IP Address     | Operating Sys |---------|--------------------| ---------------|--------------|
| Web-1     | Host a container(DVWA  |  10.0.0.5 | Linux            |
| Web-2    | Host a container(DVWA) |  10.0.0.8 | Linux            |

We have installed the following Beats on these machines:
1.Filebeat
2.Metricbeat

These Beats allow us to collect the following information from each machine:
1. Filebeat collects linux logs. This allows us to track things like user logon events, failed access attempts, service starts and stops.
2. Metricbeat collects system metrics from the web servers. We look for things like cpu usage, memory usage, drive space usage and drive space available for each host.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
- Copy the Ansible.cfg file to etc/ansible.
- Update the host file to include the IP addresses of the VMs.
- Run the playbook, and navigate to Ansible/yml file to check that the installation worked as expected.
_TODO: Answer the following questions to fill in the blanks:_

- _Which file is the playbook? Where do you copy it?_ the hosts file
- _Which file do you update to make Ansible run the playbook on a specific machine? 

How do I specify which machine to install the ELK server on versus which to install Filebeat on?_

- _Which URL do you navigate to in order to check that the ELK server is running?

###-As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

http://[your.ELK-VM.External.IP]:5601/app/kibana. In this case http://20.106.148.92:5601/app/kibana.

1. ssh azdmin@JumpBox(PrivateIP):  in this case ssh  azdmin@52.151.1.114
2. sudo docker container list -a (This will list the ansible containers. In this case, the suspicious_darwin)
3. sudo docker start (to start suspicious darwin)
4. sudo docker attach (suspicious_darwin)
5. cd /etc/ansible
6. ansible-playbook elk-playbook.yml (Installs and Configures ELK-Server)
7. cd /etc/ansible/
8. ansible-playbook firebeats-playbook.yml (Installs and configures firebeats)
9. cd /etc/ansible
10. ansible-playbook metricbeats-playbook.yml (Installs and configures metricbeats)
11. Open a new browser on Personal Workstation, navigate to (ELK-Server-PublicIP:5601/app/kibana) - This will bring up Kibana Web Portal
