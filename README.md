# IliyasElkProject
 
 ## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

![Untitled Diagram (1)](https://user-images.githubusercontent.com/83253408/130800705-a8081a9b-f1c1-469b-8d6e-7a312040b1d0.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

https://github.com/iliyas248/IliyasElkProject/blob/main/ELK_playbook.yml.txt

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
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.
Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
Load balancers help ensure environment availability through distribution of incoming data to web servers. Jump boxes allow for more easy administration of multiple systems and provide an additional layer between the outside and internal assets.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the filesystem and system performance. The ELK stack is in a separate region and virtual network. This helps separate operational services with production services.

Filebeat watch the logs,collect log evernt. Metricbeat watch the system performance (CPU,MEMORY,Network Traffic,etc...).

The configuration details of each machine may be found below.

| Name     	| Function                                     	| IP Address 	| Operating System     	|
|----------	|----------------------------------------------	|------------	|----------------------	|
| Jump Box 	| Gateway                                      	| 10.0.0.4   	| Linux (ubuntu 20.04) 	|
| Web-1    	| Host a Container DVWA                        	| 10.0.0.5   	| Linux (ubuntu 20.04) 	|
| Web-2    	| Host a Container DVWA                        	| 10.0.0.8   	| Linux (ubuntu 20.04) 	|
| ELK      	| Monitoring & log aggregation for Webs 1 & 2  	| 10.1.0.5   	| Linux (ubuntu 20.04) 	|
|          	|                                              	|            	|                      	|

### Access Policies
The machines on the internal network are not exposed to the public Internet.

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: (Personal IP Address)

Machines within the network can only be accessed by Jump Box or ELK Machine via SSH by
Jump box ( 10.0.0.4) and ELK Machine  ( 10.1.0.5)

A summary of the access policies in place can be found in the table below.
| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | My hom public IP     |
| Web-1    | No                  | Local Vnet           |
| Web-2    | No                  | Local Vnet           |
| ELK      | Yes                 | mY HOME PUBLIC IP    |

### Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because multiple servers can easily and quickly be deployed without having to physically touch each server. Going to every machine and configuring individually can be time consuming and tedious

The playbook implements the following tasks:

 1.Configue the Webservers.
 2.Install ELK
 3.Install Filebeat
 4.Install Metricbeat

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
![sudodockerps_screenshot](https://user-images.githubusercontent.com/83253408/130787332-fa35720e-88fd-4615-9cb0-acd1dbc5eaad.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

| Name  	| Function              	| IP Address 	| Operating System     	|
|-------	|-----------------------	|------------	|----------------------	|
| Web-1 	| Host a Container DVWA 	| 10.0.0.5   	| Linux (ubuntu 20.04) 	|
| Web-2 	| Host a Container DVWA 	| 10.0.0.8   	| Linux (ubuntu 20.04) 	|

We have installed the following Beats on these machines:
1.Filebeat
2.Metricbeat

These Beats allow us to collect the following information from each machine:
1. Filebeat collects linux logs. This allows us to track things like user logon events, failed access attempts, service starts and stops.
2. Metricbeat collects system metrics from the web servers. We look for things like cpu usage, memory usage, drive space usage and drive space available for each host.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
- Copy the filebeat-cfg.yml file to etc/ansible.
- Update the host file to include the IP addresses of the VMs.
- Run the playbook, and navigate to ELK-Server-PublicIP:5601/app/kibana to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it? 

The playbook files are 
elk-playbook.yml - used to install ELK Server
filebeat-playbook.yml - Used to install and configure Filebeat on Elk Server and DVWA servers
metricbeat-playbook.yml - Used to install and configure Metricbeat on Elk Server and DVWA servers

They are copied at /etc/ansible/

- _Which file do you update to make Ansible run the playbook on a specific machine? 
 
The host file. The file path is /etc/ansible/hosts.cfg

- _How do I specify which machine to install the ELK server on versus which to install Filebeat on?_

In /etc/ansible/hosts, you will specify where you want each to be installed

- _Which URL do you navigate to in order to check that the ELK server is running?

The URL is http://[your.ELK-VM.External.IP]:5601/app/kibana. In this case http://20.106.148.92:5601/app/kibana.

-As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

1. ssh to azdmin@JumpBox(PrivateIP):  in this case ssh azdmin@52.151.1.114
2. sudo docker container list -a (This will list the ansible containers. In this case, the suspicious_darwin)
3. sudo docker start suspicious_darwin
4. sudo docker attach suspicious_darwin
5. cd /etc/ansible
6. ansible-playbook elk-playbook.yml (Installs and Configures ELK-Server)
7. cd /etc/ansible/
8. ansible-playbook filebeat-playbook.yml (Installs and configures filebeat)
9. cd /etc/ansible
10. ansible-playbook metricbeat-playbook.yml (Installs and configures metricbeat)
11. Open a new browser on Personal Workstation, navigate to (ELK-Server-PublicIP:5601/app/kibana) - This will bring up Kibana Web Portal
![Filebeat_logs](https://user-images.githubusercontent.com/83253408/130902393-99a91027-40d7-444a-a634-50f53e2c0f76.png)

### References

Elasticsearch, Logstash, Kibana (ELK) Docker image documentation. (n.d) Retrieved August 12, 2021, from https://elk-docker.readthedocs.io

Virtual network peering. (n.d). Retrieved August 12, 2021, from https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview 

filebeat: Lightweight Log Analysis & Elasticsearch. (n.d.). Retrieved August 13, 2021, from https://www.elastic.co/beats/filebeat 

Metricbeat: Lightweight Shipper for Metrics. (n.d.). Retrieved August 13, 2021, from https://www.elastic.co/beats/metricbeat
