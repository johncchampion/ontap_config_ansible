## Role Configuration of ONTAP 9 with Ansible and NetApp ONTAP Collection

### Description
This is an Ansible role for configuring a NetApp ONTAP 9.6 or later storage system using the NetApp ONTAP collection. The goal is to completely configure the system leveraging a SINGLE ROLE using a vars file to accomplish the needed tasks. Unlike many roles, the tasks/main.yml is used to invoke each task (include_task) for each step of the workflow, utilizing looping structures when required. This allows each task to be individually written, reducing complexity, and hopefully making it easier to read, modify, and maintain.

This role is idempotent and can be run repeatedly.

The 'default/main.yml' is well commented, covering each var with a description, usage, and examples. Use this file to build a system-specific vars file for input and modify to suit your needs. If a variable is not set (ie. disk_assign == none) then the relevant task is not invoked. The 'defaults/main.yml' has most of the vars unset so they can be simply omitted from the provided vars input file - Ansible will use the default setting.  There are a few true/false vars with a default value (usually) set to NOT invoke the task. If the SAME setting will always be used, change the 'default/main.yml', otherwise set in a separate vars file and pass to the playbook.

### Disclaimer
The role and associated components are provided as-is and are intended to provide an example of utilizing the ONTAP collection in Ansible. Fully test in a non-production enviornment before implementing. Feel free to utilize/modify any portion of code for your specific needs.

### Requirements
* ONTAP 9.6 or later and cluster setup has been run successfully
* Ansible 2.9.7 or later
* NetApp Library: netapp-lib (pip3 install --upgrade netapp-lib)
* Ansible Galaxy Collection: netapp.ontap (https://galaxy.ansible.com/netapp/ontap)
* This role copied to the desired location (ie. ~/roles or Ansible configured roles path) 

### NOTE
**The NetApp ONTAP collection (netapp.ontap) includes several roles and there are related articles available on netapp.io. The roles are located in '{installation_path}/collections/ansible_collections/netapp/ontap/roles'.**

Roles include:
* na_ontap_cluster_config
* na_ontap_nas_create
* na_ontap_san_create
* na_ontap_snapmirror_create
* na_ontap_vserver_create

### Dev/Test Environment
* CentOS 8.1
* Ansible 2.9.7
* netapp-lib 
* netapp.ontap collection 20.04.1
* FAS2552 running ONTAP v9.7
* ONTAP Select v9.7

### Usage
* Example: **ansible-playbook pb_ontap_config.yml -e "@vars_ontap.yml"**
* Implement password security in compliance with the environment and best practices (separate vars file, ansible-vault, etc)
* A sample playbook and vars files are provided as a reference.

### Workflow
1. Gather existing cluster information (ontapinfo.yml)
2. Configure service processors (service_processor.yml)
3. Assign unowned disks to node(s) (assign_disks.yml)
4. Enable Cluster High Availability (ha_enable.yml)
5. Disable AutoSupport (disable_autosupport.yml)
6. Enable CDP (enable_cdp.yml)
7. Zero spare disks (zero_spares.yml)
8. Add licenses (licenses.yml)
9. Add login banner to cluster (loginbanner.yml)
10. Add Message of the Day (MOTD) to cluster (motd.yml)
11. Set Time Zone (timezone.yml)
12. Configure NTP (ntp.yml)
13. Configure cluster DNS (dns.yml)
14. Set SNMP Community (snmp.yml)
15. Remove specific ports from Default broadcast domain (remove_ports.yml)
16. Set specific ports flowcontrol to 'none' (flowcontrolnone.yml)
17. Create efficiency policies and schedules (efficiency_policies_schedules.yml)
18. Create interface groups (igroup.yml)
19. Add VLANs (vlans.yml)
20. Create broadcast domains (broadcast_domain.yml)
21. Create aggregates (aggregate.yml)
22. Create SVMs (svm.yml)
23. Add login banner to SVMs (loginbanner.yml)
24. Configure DNS for each SVM (dns.yml)
25. Create intercluster LIFs (lif.yml)
26. Create data and/or management LIFs (lif.yml)
27. Configure CIFS servers (cifs_server.yml)
28. Create NAS volumes (volume_nas.yml)
29. Create iGroups (igroup.yml)
30. Create SAN volumes (volume_san.yml)
31. Create and Map SAN LUNs (lun.yml)
32. Rename nodes (rename_node.yml)
33. Rename root aggregates (rename_root_aggr.yml)
34. Create Load Sharing Mirrors (lsmirror.yml)
35. Save configuration to file (save_ontap_info.yml)

### Example Playbook
- hosts: localhost
  connection: local
  gather_facts: no
  collections:
    - netapp.ontap
  tasks:
  - include_role:
      name: ontap_config

To pass a vars file to the playbook:

   ansible-playbook pb_ontap_config.yml -e "@vars_ontap_select.yml"

The 'tasks/main.yml' calls the required tasks based on the vars file settings

### Comments
Development of this role is ongoing and new tasks are added regularly. Some may end up as separate roles.

Here's a list of tasks currently being worked on:
* Cluster setup (URI module and ONTAP REST API)
* Set date and time based on Ansible controller
* Firewall settings
* FC/FCoE
* Security hardening (FIPs, MFA, Certificates, Encryption)
* Cluster Identity settings
* QoS
* Snap-related functions (Snapshots, SnapMirror)
* AIQ Unified Manager REST API (dynamic inventory?)
* Storage system reset
* Node setup (expect) prior to cluster setup (assign IPs, set admin password)
* Excel spreadsheet as input file
* Saved na_ontap_info JSON from existing system to build additional systems (in combination with unique settings in vars file)
