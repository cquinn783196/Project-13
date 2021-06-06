# Project-13
## Casey Quinn Unit 13 Project Submission
### Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Project 13 Network Diagram-Page-1](https://user-images.githubusercontent.com/77703892/120909342-1c4ece80-c642-11eb-976e-a725a27bcc2f.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Install-ELK.yml file may be used to install only certain pieces of it, such as Filebeat.

  - _root@d73dddb2df50:/etc/ansible/roles# cat filebeat-playbook.yml._

###This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
- Load Balancers: are useful because they work to process incomming traffic to be shared by bout of our vulnerable web servers.
- Jump Box: The advantage of having a jump box is because it serves as the buffer between the web and the rest of our network, and access controls are used to ensure that only authorized users are able to connect in the first place. 

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
| ELKvm    | Monitoring  | 10.1.0.4   | Linux            |

Additionally, our Azure environment has provisioned a load balancer in front of all machines except our JumpBox Provisioner. The load balancer's targets are organized into the following availability zones:
- Zone 1: Web-1 + Web-2
- Zone 2: ELKvm

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 72.92.80.78

Machines within the network can only be accessed by our Ansible within the Jumpbox-Provisioner.
- Our local machine is the only machine capable of using SSH to get into the Jumpbox-Provisioner. 
- _Jumpbox-Provisioner is the only machine within our netwrok that can access the ELKvm from the Private IP Address of 10.0.0.4

![hosts file](https://user-images.githubusercontent.com/77703892/120907178-73967400-c62d-11eb-9a64-4dc81bd608d6.PNG)


_A summary of the access policies in place can be found in the table below._

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |  72.92.80.78         |
| Web-1    | No                  |  10.0.0.4            |
| Web-2    | No                  |  10.0.0.4            |
| ELKvm    | No                  |  10.0.0.4            |

### Elk Configuration
![ELKvm example](https://user-images.githubusercontent.com/77703892/120906884-1ef1f980-c62b-11eb-835f-aa3673ccb711.PNG)

The ELK Virtual Machine exposes an Elastic Stack instance. Docker.io is used to download and manage an ELK Container. 

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because so long as the Ansible Playbooks are setup correctly, this is easily duplicated across any number of machines an ensures all of the commands and rules are installed correctly across machines. 
-Ansible Playbooks are reusable and allow us to accomplish tasks across the network without manually setting them up._

_The playbook implements the following tasks:_ 
- The ELK installation play consists of a few different elements in order to get our ELK stack up and running. 
- First, we set the elk group as our host, which is only configured for our new ELKvm and no other machines.
- Then we create taks which achieve the following
    - vn.max_map_count incraeses the amount of memory used by the ELK container
    - Install apt packages:
        - docker.io: the Docker engine which is used tto run the containers
        - python3-pip: the package used to install the Python software
    - Install docker pip packages required by Ansible to control the state of the Docker containers
    - Download the sebp/elk:761 Docker container. sebp created the container, elk is the container, 761 is the version
    - Configure the container to start with these mapping ports:
        - 5601:5601
        - 9200:9200
        - 5044:5044
    - We also want to use Ansible's sysctl module and configure it to run automatically in the event our VM gets restarted

Once our Ansbible playbook has been created to install and configure the ELK container, the play can be run on the new VM which was can verify using the docker ps command.  

The following screenshot displays the results of running docker ps after successfully configuring the ELK instance.
![In ELKvm switch to root user and check status of elk container confirmed up and running](https://user-images.githubusercontent.com/77703892/120907061-8492b580-c62c-11eb-866b-d43099a085e7.PNG)

Once we've confirmed that the ELK container is up and running, we add the ELK stack's IP address to our network security group to allow TCP traffic over port 5601 from the local machine's public IP address. 

To verify that we were able to access the server, you can navigate to http://52.150.20.43:5601/app/kibana and can see the webpage below:
![confirmation of ELK running and stable connection to Kibana](https://user-images.githubusercontent.com/77703892/120907043-501ef980-c62c-11eb-938d-7096d5287878.PNG)

_The Playbook is duplicated below:_ 
![Install Elk yml](https://user-images.githubusercontent.com/77703892/120906926-6a0c0c80-c62b-11eb-9eed-cb0bf6f123fc.PNG)

## Project Milestones thus far
- So far we've been able to deploy the new ELKvm on our virtual network, create an Ansbile play to install and configure an ELK instance and restrict access to the new server.

## Filebeat Installation
Now that our ELK monitoring server is confirmed up and running, we want to add another tool called Filebeat which will help us collect, parse, and visualize ELK logs in a single command to help us better track organizational goals. 

Below, you can see the Filebeat Playbook which is created on the Ansible VM and is then run to install Filebeat on both of our DVWAs Web-1 and Web-2 VMs at 10.0.0.5 and 10.0.06. 

![FILEBEAT ROLES ANSIBLE PLAYBOOK](https://user-images.githubusercontent.com/77703892/120907229-d7b93800-c62d-11eb-8082-e9094de10c77.PNG)

The screenshot displayed below shows that we have successfully installed Filebeat on both of our Vulnerable Web VMs. 
![filebeat-playbook in terminal success](https://user-images.githubusercontent.com/77703892/120907230-d7b93800-c62d-11eb-91e9-5f0ab2be98ee.PNG)

Then we navigate back to Kibana and ensure that log data is flowing from the vulnerable VMs on the ELK server GUI where we click Module Status to check data and verify that there is in fact incomming data.
![filebeat running and connected in kibana dashboard](https://user-images.githubusercontent.com/77703892/120907357-ad1baf00-c62e-11eb-83e7-db0afffd2f94.PNG)

# Kibana Investigation

As depicted in the image below, we're analyzing one of the recent logs from the Web-2 machine in Kibana:
![Kibana log data from web-2 vm](https://user-images.githubusercontent.com/77703892/120908661-6a5fd400-c63a-11eb-8480-e551480994cc.PNG)

Kibana allows us to have a visual represnetation of the data and events logged by on our network, giving us the ability to filter for specific instances, the types of data being transferred and where things are coming and going from. This way, we can detect any anomolies and gather the necessary data in the event of a security incident or attack. 


## PROJECT 13 MILESTONES
- Ansible Playbook successfully deployed to configure Filebeat and able to run on both vulneratble Web VMs
- Ensure that Filebeat was properly installed and running on Web-1 and Web-2 VMs
- Verify that our ELK server is receiving logs from both of the DVWA VMs


