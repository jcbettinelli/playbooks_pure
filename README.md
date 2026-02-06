
## Overview
This project demonstrates how to automate the integration of **Pure Storage FlashArray** with **VMware vSphere** using **Ansible**.  
The automation covers:
- Creating a volume on a Pure Storage FlashArray.
- Configuring FC initiators and mapping the volume.
- Replicating the volume to a secondary FlashArray.
- Rescanning ESXi HBAs and mounting the volume as a VMware datastore.
- Creating a new host and mapping an existing volume.
- Creating a snapshot and a clone from that snapshot, then mapping the clone to a host.


## Project Structure
    project/
    ├── playbook-create-datastore.yml    # Creates volume, replicates, mounts datastore
    ├── playbook-map-volume.yml          # Creates host and maps existing volume
    ├── playbook-snapshot-clone.yml      # Creates snapshot, clone, and maps clone
    ├── roles/
    │   ├── purestorage/
    │   │   ├── tasks/
    │   │   │   ├── create_volume.yml
    │   │   │   ├── configure_initiators.yml
    │   │   │   ├── replicate_volume.yml
    │   │   │   ├── create_host.yml
    │   │   │   ├── map_existing_volume.yml
    │   │   │   ├── create_snapshot.yml
    │   │   │   ├── create_clone.yml
    │   │   │   ├── map_clone_to_host.yml
    │   │   └── defaults/main.yml
    │   ├── vmware/
    │   │   ├── tasks/
    │   │   │   ├── rescan_hba.yml
    │   │   │   ├── mount_datastore.yml
    │   │   └── defaults/main.yml
    └── group_vars/
    └── all.yml

---
## Playbooks
### **1. `playbook-create-datastore.yml`**
- Creates a new volume on FlashArray A.
- Configures FC initiators and maps the volume.
- Replicates the volume to FlashArray B.
- Rescans ESXi HBAs and mounts the volume as a VMware datastore.

#### Run:
    ansible-playbook playbook-create-datastore.yml

### **2. `playbook-map-volume.yml`**

- Creates a new host on FlashArray.
- Maps an existing volume to that host.

#### Run:

    ansible-playbook playbook-map-volume.yml

### **3. `playbook-snapshot-clone.yml`**

- Creates a snapshot of an existing volume.
- Creates a clone from that snapshot.
- Maps the clone to a host.

#### Run:

    ansible-playbook playbook-snapshot-clone.yml

## Prerequisites
1. **Install Ansible Collections:**
   ```bash
   ansible-galaxy collection install purestorage.flasharray
   ansible-galaxy collection install community.vmware
2. **Install Python Dependencies:**
   ```bash
   pip install purestorage pyvmomi py-pure-lcient
3. **Credentials:**
- FlashArray API tokens
- vCenter credentials
- Store these securely using Ansible Vault or environment variables.




## How to Run

1. Export environment variables (if using lookup('env')):
    ```bash
    export FA_A_URL="flasharray-a.example.com"
    export FA_A_API_TOKEN="your_api_token_a"
    export FA_B_URL="https://flasharray-b.example.com"
    export FA_B_API_TOKEN="your_api_token_b"
    export VCENTER_HOSTNAME="vcenter.example.com"
    export VCENTER_USERNAME="administrator@vsphere.local"
    export VCENTER_PASSWORD="your_vcenter_password"
2. Execute any playbook:
    ```bash
    ansible-playbook playbook.yml
## Features
- Modular roles for Pure Storage and VMware.
- Supports Fibre Channel initiators (WWNs).
- Secure credential handling via Vault or environment variables.
- Includes HBA rescan before datastore creation for VMware.
## Best Practices
- Use Ansible Vault for sensitive data:
    ```bash
    ansible-vault encrypt_string 'mysecret' --name 'fa_a_api_token'
- Validate zoning and connectivity for FC before running.

- Test in a non-production environment first.  


