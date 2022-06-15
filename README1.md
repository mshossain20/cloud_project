## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/mshossain20/cloud_project/blob/main/diagram/NETWORK%20DIAGRAM.png) 


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Yaml file may be used to install only certain pieces of it, such as Filebeat and metricbeat.

Enter the playbook file._
  Install-elk.yml
  filebeat-configuration.yml
  filebeat.yml
  metricbeat-configuration.yml
  metricbeat.yml
  

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
 What aspect of security do load balancers protect? 
A load balancer can add additional layers of security to our website without any changes to our application.This helps distribute traffic evenly across the servers and mitigates DoS attacks.It Protect applications from emerging threats, Authenticate User Access,and Simplify PCI compliance. The major goal of the load balancer, according to the Azure security baseline for Azure Load Balancer, is to distribute web traffic across numerous servers. To safeguard Azure resources within virtual networks, a load balancer was built in front of the VM on our network to

o protect Azure resources within virtual networks.
o monitor and log the configuration and traffic of virtual networks, subnets, and NICs.
o protect critical web applications
o deny communications with known malicious IP addresses
o record network packets
o deploy network-based intrusion detection/intrusion prevention systems (IDS/IPS)
o manage traffic to web applications
o minimize complexity and administrative overhead of network security rules
o maintain standard security configurations for network devices
o document traffic configuration rules
o use automated tools to monitor network resource configurations and detect changes

What is the advantage of a jump box?

A jump box is a network solution that allows users to access and manage devices in a secure zone. A jump server is a hardened and monitored device that connects two different security zones and allows for controlled access. It serves as a "bridge" between two trusted network zones, allowing for regulated access. The public IP address linked with the VM can be blocked. It improves security by preventing all Azure VMs from being exposed to the public. We can simply monitor the susceptible VMs for changes to their file systems and system metrics like privilege escalation failures, SSH login activity, CPU and RAM use, and so on by integrating an Elastic Stack server.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data  and system logs.
 What does Filebeat watch for?_

Filebeat is a small shipper for forwarding and storing log data. Filebeat is a server-side agent that monitors the log files or locations we  designate, collects log events, and sends them to Elasticsearch or Logstash for indexing. It simplifies things by providing a lightweight (low memory footprint) means to forwards and consolidate logs, files, and change watches.

What does Metricbeat record?_
Metricbeat collects metrics and statistics and sends them to the destination you select, such as Elasticsearch or Logstash. Metricbeat collects information from the system and services operating on the server, such as Apache, to help you manage your servers. HAProxy. Metricbeat is a server monitoring tool that collects data from the system and services operating on the server in order to record machine metrics and stats like uptime. Below are the configuration details for each machine.



| Name               | Function      | IP Address   | Operating System |
|--------------------|---------------|--------------|------------------|
| Jumpboxprovisioner | Gateway       | 20.28.148.48 | Linux            |
| Web-1              | webserver     | 10.1.0.8     | Linux            |
| Web-2              | Webserver     | 10.1.0.9     | Linux            |
| ElkVM              | Kibana        | 10.2.0.4     | Linux            |
| Bootcamp1-LB       | load balancer | 20.58.164.11 | DVWA             |

In addition to the above, Azure has provisioned a load balancer in front of all machines except for the jump box. The load balancer's targets are organized into availability zones: Web-1 + Web-2

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpboxprovisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
 20.58.164.11 Machines within the network can only be accessed by SSH from Jumpboxprovisioner.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_

A summary of the access policies in place can be found in the table below.
| Name               | Publicly accessible | Allowed IP address |
|--------------------|---------------------|--------------------|
| Jumpboxprovisioner | Yes                 | 20.28.148.48       |
| ELKVM              | No                  | 10.2.0.4           |
| DVWA1              | No                  | 10.1.0.8           |
| DVWA2              | No                  | 10.1.0.9           |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
What is the main advantage of automating configuration with Ansible?_
It's really easy to set up and use: Ansible's playbooks don't require any special coding knowledge (more on playbooks later). Powerful: Even the most complex IT procedures may be modeled with Ansible. Flexible: You can manage the complete application environment, regardless of where it's installed. This allows us to use a single playbook to deploy to numerous servers.

The playbook implements the following tasks:
Install docker.io
Install Python-pip
Install docker container
Launch docker container: elk
Command: sysctl -w vm.max_map_count=262144

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker ps]("C:\Users\Spectre\Desktop\New folder\docker ps.docx") 


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
Web1 and Web-2
List the IP addresses of the machines you are monitoring_
Web-1 IP address : 10.1.0.8
Web-2 Ip address :10.1.0.9

We have installed the following Beats on these machines:
Specify which Beats you successfully installed_
Filebeat and metricbeat.

These Beats allow us to collect the following information from each machine:

Filebeat keeps an eye on the log files or locations we designate, gathers log events, and sends them to Elasticsearch or Logstash for indexing. - Metricbeat gathers data from the operating system as well as server-side services.

### Using the Playbook
In order to use the playbook, we will need to have an Ansible control node already configured. Assuming we have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /etc/ansible.
- Update the configuration file to include the webservers and ElkVM private Ip address. 
- Run the playbook, and navigate to ELKVM to check that the installation worked as expected.
/etc/ansible/host should include: [webservers]
10.1.0.8  ansible_python_interpreter=/usr/bin/python3
10.1.0.9  ansible_python_interpreter=/usr/bin/python3
[elk]
10.2.0.4  ansible_python_interpreter=/usr/bin/python3


ansible_python_interpreter=/usr/bin/python3 [10.1.0.8] ansible_python_interpreter=/usr/bin/python3 [10.1.0.9] , ansible_python_interpreter=/usr/bin/python3 [elk] [10.2.0.4] ansible_python_interpreter=/usr/bin/python3 Run the playbook, and SSH into the Elk vm, then run docker ps to check that the installation worked as expected. Playbook: install_elk.yml Location: /etc/ansible/roles/install_elk.yml Navigate to http://[your.ELK-VM.External.IP]:5601/app/kibana to confirm ELK and kibana are running. We may need to try from multiple web browsers Click 'Explore On Your Own' and you should see the following:

:[kibana]("C:\Users\Spectre\Desktop\New folder\kibana.docx")

Using the Playbook-filebeat-playbook.yml - Copy the filebeat.yml file to the /etc/ansible/files/ directory. - Update the configuration file to include the Private IP of the Elk-Server to the ElasticSearch and Kibana sections of the configuration file. - Create a new playbook in the /etc/ansible/roles/ directory that will install, drop in the updated configuration file, enable and configure system module, run the filebeat setup, and start the filebeat service. - Create a new playbook in the /etc/ansible/roles/ directory that will install, drop in the updated configuration file, enable and configure system module, run the metricbeat setup, and start the metricbeat service. - Run the playbooks, and navigate back to the installation page on the ELk-Server GUI, click the check data on the Module Status - Click the verfiy incoming Data to check and see the receiving logs from the DVWA machines. you should see the following:

:[filebeat-ecs]("C:\Users\Spectre\Desktop\New folder\filebeat.ecs.docx")

The commands needed to run the Ansible configuration for the Elk-Server are: - ssh azdmin@JumpBox(Public IP) - sudo docker container list -a (locate your ansible container) - sudo docker start container (name of the container) - sudo docker attach container (name of the container) cd /etc/ansible/ - ansible-playbook install-elk.yml (configures Elk-Server and starts the Elk container on the Elk-Server) wait a couple minutes for the implementation of the Elk-Server - cd /etc/ansible/roles/ - ansible-playbook filebeat.yml (installs Filebeat and Metricbeat) - open a new web browser (http://[your.ELK-VM.External.IP]:5601/app/kibana) This will bring up the Kibana Web Portal - check the Module status for file beat and metric beat to see their data receiving. ** You will need to ensure all files are properly placed before running the ansible-playbooks. 



_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._