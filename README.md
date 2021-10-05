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

### Dev/Test Environment
* CentOS 8.4.2105
* Ansible Core 2.11.5
* netapp-lib 
* netapp.ontap collection 21.11
* FAS2552 running ONTAP v9.8P3
* ONTAP Select v9.8P6 & v9.9.1P2
* vSphere 6.7 & 7.0

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
16. Create efficiency policies and schedules (efficiency_policies_schedules.yml)
17. Create interface groups (igroup.yml)
18. Add VLANs (vlans.yml)
19. Create broadcast domains (broadcast_domain.yml)
20. Create aggregates (aggregate.yml)
21. Create SVMs (svm.yml)
22. Add login banner to SVMs (loginbanner.yml)
23. Configure DNS for each SVM (dns.yml)
24. Create intercluster LIFs (lif.yml)
25. Create data and/or management LIFs (lif.yml)
26. Configure CIFS servers (cifs_server.yml)
27. Create NAS volumes (volume_nas.yml)
28. Create iGroups (igroup.yml)
29. Create SAN volumes (volume_san.yml)
30. Create and Map SAN LUNs (lun.yml)
31. Rename nodes (rename_node.yml)
32. Rename root aggregates (rename_root_aggr.yml)
33. Create Load Sharing Mirrors (lsmirror.yml)
34. Save configuration to file (save_ontap_info.yml)

### Example Playbook
<pre>
- hosts: localhost
  connection: local
  gather_facts: no
  collections:
    - netapp.ontap
  tasks:
  - include_role:
      name: ontap_config
</pre>

To pass a vars file to the playbook:

   **ansible-playbook pb_ontap_config.yml -e "@vars_ontap_select.yml"**

The 'tasks/main.yml' calls the required tasks based on the vars file settings
