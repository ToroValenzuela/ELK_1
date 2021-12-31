# ELK_1
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Images/Project_1lab.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the pentest.yml file may be used to install only certain pieces of it, such as Filebeat.

---
- name: Config Web VM with Docker
  hosts: webservers
  become: true
  tasks:
  - name: docker.io
    apt:
      force_apt_get: yes
      update_cache: yes
      name: docker.io
      state: present

  - name: Install pip3
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

  - name: Install Docker python module
    pip:
      name: docker
      state: present

  - name: download and launch a docker web container
    docker_container:
      name: dvwa
      image: cyberxsecurity/dvwa
      state: started
      published_ports: 80:80

  - name: Enable docker service
    systemd:
      name: docker
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

Load balancing ensures that the application will be highly secure, in addition to restricting external traffic to the network.
Load balancers distributes network traffic across number of servers, also load balancers defends an organization against Denial of Service attacks (DDoS). Jump-box VM is a system prevent the exposition of all the virtual machines to the public, also is used to access and manage devices in a separate security zone.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system performance.
-Filebeat collects data about the system and enables analysts to monitor files for suspicious changes.
-Metricbeat record specific information about the machines in the network.

The configuration details of each machine may be found below.


| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux Ubuntu18.04|
| Web-1    |          | 10.0.0.5   | Linux Ubuntu18.04|           
| Web-2    |          | 10.0.0.6   | Linux Ubuntu18.04|           
| ElkVM    |          | 10.1.0.4   | Linux Ubuntu18.04|          

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
10.0.0.5, 10.0.0.6 and 68.116.82.41

Machines within the network can only be accessed by .
Elk VM can be accessed by 10.0.0.4 (Jump-Box-Provisioner)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | 10.0.0.5 10.0.0.6    |
| Web      | No                  | 10.0.0.4             |
| ELK      | No                  | 10.0.0.4             |           

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it saves time and avoid errors, because it handles configuration management and it is capable to manage machines quickly and in parallel focusing in security giving the opportunity of an easy audibility/review/rewriting of content.

The playbook implements the following tasks:
- Create a new Virtual Network for ELK.
- Create a Peer Network Connection between the two vNets.
- Create a new VM. Deploy a new VM in to the vNet with its own Security Group. This VM will host the ELK server.
- Download and configure the elk-docker container onto the ELK server VM.
- Launch and expose the elk-docker container to start the ELK server.
- Implement identity and access management configuring the ELK security group to be able to connect ELK via HTTP, and view it through.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Images/docker_ps_output.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.5
10.0.0.6

We have installed the following Beats on these machines:
Filebeat on 10.0.0.5, 10.0.0.6 and DVWA VM's.
Metricbeat on 10.0.0.5, 10.0.0.6 and DVWA VM's. 

These Beats allow us to collect the following information from each machine:
Filebeat: collects data about the filesystem.
Metricbeat: collects machine metrics, such as uptime.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to install and configure Filebeat on /etc/ansible/roles/
- Update the filebeat-config.yml file to include the Web VMs
- Run the playbook, and navigate to http://[VM_IP]:5601/app/kibana to check that the installation worked as expected.

Commands: 
dpkg -i filebeat-7.4.0-amd64.deb (to install the .deb file)
cp fielebeat-playbook.yml /etc/fielebat/fielebat.yml

filebeat modules enable system
filebeat setup
service filebeat start

curl -o to download the dpkg file
