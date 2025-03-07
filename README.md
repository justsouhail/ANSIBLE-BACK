# Ansible-Backup
![Project Logo](téléchargement.png)



## Table of Contents
- [Introduction](#introduction)  
- [Topology](#topology)  
- [Steps](#steps)  
- [Device IP Address Table](#device-ip-address-table)  
- [Hosts Inventory](#hosts-inventory)  
- [Fortigate](#fortigate)  



## Introduction
I'm developing an Ansible-based backup automation project for firewalls (Fortinet, Palo Alto, F5) and network devices (Cisco switches and routers). The project includes a web interface to manage and monitor the automation process. Initially, I'm using PNetLab for simulation, with plans to transition to real-world scenarios for deployment and testing.


## Topology 
![Project Logo](assets/pnetlogo.png)


## Steps

### 🔹 **Phase 1: Environment Setup**  
- [ ] Set up **PNetLab** with firewall and network device images  
- [ ] Install **Ansible** and required dependencies  
- [ ] Configure SSH & API access for devices  
- [ ] Define Ansible inventory for network devices  

### 🔹 **Phase 2: Backup Automation**  
- [ ] Create Ansible playbooks for:  
  - [ ] **Fortinet** backup  
  - [ ] **Palo Alto** backup  
  - [ ] **Cisco switches/routers** backup
  - [ ] **Dell switches/routers** backup
  - [ ] **F5** backup  
- [ ] Implement a  FTP  solution  
- [ ] Schedule automated backups

### 🔹 **Phase 3: Web Interface Development**  
- [ ] Design a **dashboard** for backup management  

### 🔹 **Phase 4: Testing & Real-World Deployment**  
- [ ] Validate automation in **PNetLab**  
- [ ] Deploy to real devices in a **test environment**


## Device IP Address Table  

| Device Type   | Vendor     | Hostname     | IP Address     |
|--------------|-----------|-------------|--------------|
| Firewall     | Fortinet   | FW1  | 192.168.11.66  |
| Firewall     | Palo Alto  | PA-VM-1      | 192.168.11.160  |
| Firewall     | F5         | F5-LTM-1     |   |
| Switch       | Cisco      | SW-Core-1    | 192.168.11.28 |
| Router       | Cisco      | R1           | 192.168.11.25 |
| Router       | Cisco      | R2           | 192.168.11.26 |
| Server      | Ubuntu      | Control node           | 192.168.11.100 |
| Server      | Ubuntu      |  SFTP         |  |


## Hosts inventory 
 

### **📂** Playbook Location: `playbooks/hosts`**  

```ini





[fortigates]
fgt ansible_host=192.168.11.66 ansible_user=admin ansible_password=ertdfgcvb

[fortigates:vars]
ansible_network_os=fortinet.fortios.fortios
[all:vars]

ansible_connection=httpapi

ansible_httpapi_validate_certs=no

ansible_httpapi_use_ssl=no






```



##  fortigate 

### Firewall model
![Project Logo](assets/forti.png)





### **📂** Playbook Location: `playbooks/forti.yml`**  
```yaml
---
- name: General Config
  hosts: fortigates
  gather_facts: true
  collections:
    - fortinet.fortios

  vars:
    new_hostname: "fw1"
    vdom: "root"

  tasks:

    - name: Get time
      command: date +%Ym-%d
      register: timestamp


    - name: Backup fortinet current config
      fortios_monitor_fact:
        vdom:  "{{ vdom }}"
        selector: 'system_config_backup'
        params:
          scope: "global"
      register: full_config



```




