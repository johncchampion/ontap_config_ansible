## Role Configuration of ONTAP 9 with Ansible and NetApp ONTAP Collection
-----
### Description
This is an Ansible role for configuring a NetApp ONTAP 9.6 or later storage system using the NetApp ONTAP collection. The goal is to completely configure the system leveraging a SINGLE ROLE using a vars file to accomplish the needed tasks. Unlike many roles, the tasks/main.yml is used to invoke each task (include_task) for each step of the workflow, utilizing looping structures when required. This allows each task to be individually written, reducing complexity, and hopefully making it easier to read, modify, and maintain.

This role is idempotent and can be run repeatedly.

The default/main.yml is well commented, covering each variable with a description, usage, and examples. Use this file to build a system-specific vars file for input and modify to suit your needs.  

### Requirements
* ONTAP 9.6 or later
* Ansible 2.9.7 or later
* NetApp Library: netapp-lib

     *pip3 install --upgrade netapp-lib*

* Ansible Galaxy Collection: netapp.ontap (https://galaxy.ansible.com/netapp/ontap)

     **Default path** 
     
       *ansible-galaxy collection install netapp.ontap*

     **Ansible Search path** 
     
       *ansible-galaxy collection install netapp.ontap -p /usr/share/ansible/collections -f*

     **Specific path** 

       *ansible-galaxy collection install netapp.ontap -p ./collections -f*

     **From downloaded file** 

       *ansible-galaxy collection install /tmp/netapp-ontap-20.3.0.tar.gz -p /usr/share/ansible/collections/*

* This role in desired location (ie. ~/roles or Ansible configured roles path) 

### Dev/Test Environment
* CentOS 8.1
* Ansible 2.9.7
* netapp-lib 
* netapp.ontap collection 20.04.1
* FAS2552 running ONTAP v9.7
* ONTAP Select v9.7

### Workflow



### Example Playbook



### Comments


