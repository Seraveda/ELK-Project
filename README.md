## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/118O0yPieN0aNGHHZXDVYQufQXfj8YWsD/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

pentest.yml - used to install DVWA Servers
elk.yml - used to install ELK Server
filebeat-playbook.yml - used to install and configure Filebeat on ELK Server and DVWA servers
metricbeat-playbook.yml - used to install and configure Metricbeat on ELK Server and DVWA servers


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

- Load balancing ensures that the application will be highly available, in addition to restricting access to the network. Load Balancing ensures availability to the web-servers which is the availability aspect of security in regards to the CIA Triad.

- The main advantage of using a JumpBox is having one origination point for administrative tasks. This sets the JumpBox as a Secure Admin Workstation (SAW). In order to conduct administrative tasks administrators are required to access the JumpBox before accessing the other servers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- What does Filebeat watch for? Filebeat watches for log files/locations and collect log events. (Filebeat: Lightweight Log Analysis & Elasticsearch)
- What does Metricbeat record? Metricbeat records metrics and statistical data from the operating system and from services running on the server (Metricbeat: Lightweight Shipper for Metrics)

The configuration details of each machine may be found below:

| Name       | Function                   | IP Address | Operating System        |
|------------|----------------------------|------------|-------------------------|
| Jump Box   | Gateway                    | 10.0.0.1   | Linux (Ubuntu 20.04 LTS |
| Web-1      | Web Server - Docker - DVWA | 10.0.0.5   | Linux (Ubuntu 20.04 LTS |
| Web-2      | Web Server - Docker - DVWA | 10.0.0.6   | Linux (Ubuntu 20.04 LTS |
| ELK-Server | ELK Stack	              | 10.2.0.4   | Linux (Ubuntu 20.04 LTS |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the "Jump Box Provisioner" machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 

- 52.255.159.204

Machines within the network can only be accessed by SSH. The ELK-Server is only accessible by SSH from the JumpBox and via web access from Personal IP Address

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible    | Allowed IP Addresses |
|------------|------------------------|----------------------|
| Jump Box   | No                     | Personal IP Address  |
| Web-1/2    | Yes thru Load Balancer |                      |
| ELK-Server | No                     | SSH 10.0.0.4 - JumpBox HTTP Port 5601 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantage of automating the installation process is that we could deploy multiple servers easily and quickly without having to physically touch each server.

The playbook implements the following tasks:

1. Install Docker.io and pip3
2. Increases VM memory
3. Download and Configure elk docker container
4. Set Ports

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://drive.google.com/file/d/1PiVfqXw4r-f3fcpMDQFTjVQB3u880VwJ/view?usp=sharing

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- Web-1 10.0.0.5
- Web-2 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
 - Filebeat watches for log files/locations and collect log events.
 - Metricbeat records metrics and statistical data from the operating system and from services running on the server.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml and metricbeat-config.yml file to /etc/ansible
- Update the config file to include the private IP of the ELK-Server to the Elasticsearch and Kibana sections of the config file.
- Run the playbook, and navigate to ELK-Server-PublicIP:5601/app/kibana to check that the installation worked as expected.

Which file is the playbook? Where do you copy it?
- elk-playbook.yml
- filebeat-playbook.yml
- metricbeat-playbook.yml 

Where do you copy it?
Copy to /etc/ansible/

Which file do you update to make Ansible run the playbook on a specific machine?
/etc/ansible/hosts.cfg

How do I specify which machine to install the ELK server on versus which to install Filebeat on?
In /etc/ansible/hosts specify where you want each to be installed.

Which URL do you navigate to in order to check that the ELK server is running?
http://publicip(elkserver):5601

Specific commands the user will need to run to download the playbook, update the files, etc:

1. ssh Admin@JumpBox (PrivateIP)
2. sudo docker container list -a - Locate the ansible container
3. sudo docker start (Funny_N. ame)
4. sudo docker attach (Funny_Name)
5. cd /etc/ansible
6. ansible-playbook elk.yml (Installs and Configures ELK-Server)
7. cd /etc/ansible/
8. ansible-playbook filebeat-playbook.yml (installs and configures filebeat)
9. ansible-playbook filebeat-playbook.yml (installs and configures metricbeat)
10. Open a new browser and navigate to ELK-Server-PublicIP:5601/app/kibana - This will bring up Kibana Web Portal
