# Cybersecurity Project Portfolio

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/togeworl/cybersecurity-portfolio/blob/2b013cd16c5ccc96f06877d3cf6a4cdc278b4ad5/Images/Network_Diagram.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ELK installation playbook may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access and traffic to the network. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file system and machine metrics.

The configuration details of each machine may be found below.

| Name       | Function   | IP Address | Operating System |
|------------|------------|------------|------------------|
| Jump Box   | Gateway    | 10.0.0.4   | Linux            |
| Web-1 VM   | Web server | 10.0.0.5   | Linux            |
| Web-2 VM   | Web server | 10.0.0.6   | Linux            |
| ELK Server | ELK Server | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 

2601:84:8700:85c0:79f0:94b:573b:50e7 (Local Machine)

Machines within the network can only be accessed through the JumpBox Provisioner via SSH. The JumpBox Provisioner can access all other virtual machines in the network.  

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible  | Allowed IP Addresses     |
|------------|----------------------|--------------------------|
| Jump Box   | Yes w/ SSH and white | Local Machine IP Address |                |            | listed IP address    |                          |
| Web-1 VM   | No                   | Jump Box IP 10.0.0.4     |
| Web-2 VM   | No                   | Jump Box IP 10.0.0.4     |
| ELK Server | No                   | Jump Box IP 10.0.0.4     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it reduces human error and simplifies the configuration process, allowing multiple machines to be configured efficiently, saving time and costs. 

The ELK installation playbook implements the following tasks:
- The hosts and users are designated 
- Docker is installed 
- pip3 is installed 
- Docker Python module is installed 
- More memory is also allocated to the machines 
- A Docker ELK container is downloaded and launched with images and published ports
- Docker is booted 

![ELK Playbook](https://github.com/togeworl/cybersecurity-portfolio/blob/b48a0cf59b9f0ad60947a30fc7af1f1ef5256137/Images/ELK_Playbook_YML.jpg)

The following screenshot displays the result of running `sudo docker container ls -a` after successfully configuring the ELK instance.

![List ELK Containers](https://github.com/togeworl/cybersecurity-portfolio/blob/b48a0cf59b9f0ad60947a30fc7af1f1ef5256137/Images/ELK_Docker_PS.jpg)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 VM 10.0.0.5
- Web-2 VM 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat 
- Metricbeat 

These Beats allow us to collect the following information from each machine:
- Filebeat and Metricbeat are both Elastic Beats and are based on the libbeat framework. Filebeat monitors log files or locations, collects log events, and forwards them either to Elasticsearch or Logstash for indexing. Metricbeat periodically collects metrics from the operating system and from servers running on the server and forwards them to either Elasticsearch or Logstash. Metricbeat monitors services such as Apache, MySQL, or Nginx, as well as many others. 
- For more details, the following links are provided: 
  [Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html)
  [Metricbeat](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-overview.html)


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the installation playbook file to a .yml file in the /etc/ansible directory inside the ansible container.

- Update the hosts file in the ansible directory to include the correct hosts and groups, such as a webservers group with Web-1 and Web-2 IP addresses listed; also add "ansible_python_interpreter=/usr/bin/python3" after the IP address 

![Ansible hosts file](https://github.com/togeworl/cybersecurity-portfolio/blob/b48a0cf59b9f0ad60947a30fc7af1f1ef5256137/Images/ansiblehostsfile.jpg)

- Update the installation playbook file with the correct file paths and hosts 

- Update the matching configuration .yml file with the following changes:
 
- *For Filebeat:* 
  - *Scroll to line #1106 and replace the IP address with the internal IP address of the ELK server (not the public IP)* 
  -  *Scroll to line #1806 and replace the IP address with the internal IP address of the ELK server* 

- *For Metricbeat:* 
  - *Scroll to line #62 and replace the IP address with the internal IP address of the ELK server (not the public IP); leave the port as 5601* 
  - *Scroll to line #95 and replace the IP address with the IP address of of the ELK server; leave the port as 9200* 

- Make sure to save the files after making the changes 
 
- Run the playbook, and navigate to http://[YourELKServerIP]:5601/app/kibana (in this case, the ELK server IP is 168.62.201.121, so the full link would be http://168.62.201.121:5601/app/kibana) to check that the installation worked as expected.

### Commands

To generate an SSH key: 

- $ ssh-keygen 
- If generating a key inside of an Ansible container for a webserver or other machine inside of the network, do not set a passphrase in order to enable automation of configuration 

To SSH into the JumpBox Provisioner VM from local machine: 

- $ ssh -i (full path to the private ssh key used when setting up the VMs in the Azure portal) username@ip_of_jumpbox 
  - Enter passphrase for ssh key when prompted 
  - This will take the user into the JumpBox
  - Example command: ssh -i ~/.ssh/id_rsa azadmin@52.152.235.68 

To install Docker in the JumpBox: 

- $ sudo apt install docker.io 

To pull Ansible image: 

- $ sudo docker pull cyberxsecurity/ansible 

To see what images are pulled: 

- $ sudo docker images 

To create a container: 

- $ sudo docker run -ti --name (name of container) (image) bash 
  - Can use any unique name, and include an image that has already been pulled, such as cyberxsecurity/ansible 

To start a container: 

- $ sudo docker start (name of container) 
  - Only start a container once if it is not running, otherwise it will create a new container each time it is started 

To get into a container: 

- $ sudo docker exec -ti (name of container) bash 
  - There are other commands that will do the same thing 

To see what Docker containers are available: 

- $ sudo docker container ls -a 
  - This will list all the containers with their Container ID, Image, Command, Creation Date, Status, Ports, and Names 

To run an Ansible playbook ./yml file: 

- $ ansible-playbook elk.yml 
  - Use for any Ansible playbook file 








