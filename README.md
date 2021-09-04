## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Update the path with the name of your diagram] 
GT-Cyber-bootcamp/Saurabh's_NetDiagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the __docker___ file may be used to install only certain pieces of it, such as Filebeat.

---
  - name: installing and launching filebeat
    hosts: webservers
    become: yes
    tasks:

    - name: download filebeat deb
      command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

    - name: install filebeat deb
      command: dpkg -i filebeat-7.6.1-amd64.deb

    - name: drop in filebeat.yml
      copy:
        src: /etc/ansible/files/filebeat-config.yml
        dest: /etc/filebeat/filebeat.yml

    - name: enable and configure system module
      command: filebeat modules enable system

    - name: setup filebeat
      command: filebeat setup

    - name: start filebeat service
      command: service filebeat start

    - name: enable service filebeat on boot
      systemd:
        name: filebeat
        enabled: yes
This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly __versatile___, in addition to restricting _high traffic____ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?  
-Load balancers are useful for redundancy purposes. For example, if there is a D/DOs attack or if there is a lot of traffic another server can substitute or alleviate it.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ system _____.
- _TODO: What does Filebeat watch for?_ 
-Filebeat watches and monitors the log files or locations that users specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- _TODO: What does Metricbeat record?_ 
-Metricbeat Collect metrics from your systems and services. From CPU to memory, Redis to NGINX, and much more, It is a lightweight way to send system and service statistics.


| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.9   | Linux            |
| Web-1    |  DVWA    | 10.0.0.8   | Linux            |
| Web-2    |  DVWA    | 10.0.0.7   | Linux            |
| Web-3    |  DVWA    | 10.0.0.10  | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the __Security Group___ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_
	172.125.54.185
	10.0.0.9
Machines within the network can only be accessed by __Jump box___.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?
	-Jump box has access to my ELK VM the ip address 10.0.0.9
A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | YES                 | 172.125.54.185       |
|ELK Server| YES                 | 40.71.64.81          |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
•	Install Docker.io
•	Install Docker module
•	Increase Virtual Memory count – 262144
•	Download and launch elk container sebp
•	Enable elk ports on 5601, 9200, 5044

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
	- 10.0.0.8, 10.0.0.7, 10.0.0.10

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
	- Metricbeat & Filebeat
These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
- Metricbeat periodically logs its internal metrics that have changed in the last period. For each metric that changed, the delta from the value at the beginning of the period is logged.
- Filebeat modules are ready-made configurations parsing the data and analyzing it in Kibana with dashboards.

### Using the Playbook
To use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _my-playbook.yml file to _playbook____.
- Update the ___my-playbook.yml__ file to include the host: webservers  
- Run the playbook and navigate to curl localhost/setup.php to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks: _
- _Which file is the playbook? Where do you copy it? _
-the file would be in /etc/ansible
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? _
- sudo apt install docker.io -This is where you can get the docker container
- You can install the Elk server by updating the host config to the elk server ip address and specify your playbook to update only on ELK servers.
- You can also update you host config to specify where the filebeat webservers should be.
- _Which URL do you navigate to check that the ELK server is running?
	- http://[YOUR_ELK_SERVER_IP_ADDRESS]:5601/app/kibana#/home

