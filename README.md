# Project-13
# Casey Quinn Unit 13 Project Submission
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Install-ELK.yml file may be used to install only certain pieces of it, such as Filebeat.

  - _root@d73dddb2df50:/etc/ansible/roles# cat filebeat-playbook.yml._

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
- _Load Balancers: are useful because they work to process incomming traffic to be shared by bout of our vulnerable web servers.
- _Jump Box: The advantage of having a jump box is because it serves as the buffer between the web and the rest of our network, and access controls are used to ensure that only authorized users are able to connect in the first place. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the Virtual Machines on the network and system metrics such as CPU usage, attempted SSH login attempts, sudo escalation failures, and more.
- _Filebeat: detects changes to the filesystem. Specifically, we use it to collect Apache logs_
- _Metricbeat: detects changes in system metrics, such as CPU usage. We use this to detect SSH login attempts, failed sudo escalations and CPU/RAM statistics._

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function     | IP Address | Operating System |
|----------|--------------|------------|------------------|
| Jump Box | Gateway      | 10.0.0.4   | Linux            |
| Web-1    | Web Server   | 10.0.0.5   | Linux            |
| Web-2    | Web Server   | 10.0.0.6   | Linux            |
| ELKvm    | Monitoroing  | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 72.92.80.78

Machines within the network can only be accessed by our Ansible within the Jumpbox-Provisioner.
- Our local machine is the only machine capable of using SSH to get into the Jumpbox-Provisioner. 
- _Jumpbox-Provisioner is the only machine within our netwrok that can access the ELKvm from the Private IP Address of 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |  72.92.80.78         |
| Web-1    | No                  |  10.0.0.4            |
| Web-2    | No                  |  10.0.0.4            |
| ELKvm    | No                  |  10.0.0.4            |

### Elk Configuration

The ELK Virtual Machine exposes an Elastic Stack instance. Docker.io is used to download and manage an ELK Container. 

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because so long as the Ansible Playbooks are setup correctly, this is easily duplicated across any number of machines an ensures all of the commands and rules are installed correctly across machines. 
-Ansible Playbooks are reusable and allow us to accomplish tasks across the network without manually setting them up._

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...

The following screenshot displays the results of running docker ps after successfulyl configuring the ELK instance. 
- _TODO: Update the image file pather with the name of my screenshot of docker ps output:

The Playbook is duplicated below: 
