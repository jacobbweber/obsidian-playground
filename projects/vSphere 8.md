---
Project_Name: vSphere 8
Start_Date: 2024-06-01
Due_Date: 2024-12-31
Status: Started
Priority: Medium
Active_Milestone: Milestone 1
Project_Type: Departmental
Owner/Lead: Infrastructure
Phase:
---

# Project Overview

**Project Name:** vSphere 8.0 U2 Upgrade Project Plan

**Purpose:** vSphere 7 EOL is coming up, this effort is to stay within a supported lifecycle and to take advantage of new features allowing us a more robust datacenter architecture, including Active-Active setup.

**Use Cases:** 

**Cost:**

**Timeline:** 

# Resources
- Visuals

- Docs
	- links to vsphere 8 upgrade stuff

- Primary Contacts
	 - vendors or sumtin
	 
-  Product Compatibility with vSphere 8.0U2
	- **VMware Aria Suite Lifecycle**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=1729,&row=2)
	- **VMware Aria Operations for Logs**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=1740,&row=2)
	- **VMware Aria Operations**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=1427,&row=2)
	- **VMware Aria Operations for Networks**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=1372,&row=2)
	- **VMware Aria Automation**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=1743,&row=2)
	- **VMware Aria Automation Orchestrator**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=1744,&row=2)
	- **VADP-based Backup Solution (CommVault/Veeam/Avamar)**
	- **VMware vSphere Replication**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=671,&row=2)
	- **VMware Site Recovery Manager**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=647,&row=2)
	- **VMware NSX**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=912,&row=2)
	- **VMware HCX**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=660,&row=2)
	- **vCenter Upgrade - Back-in-time restriction**: [KB Article](https://kb.vmware.com/s/article/67077)
	- **Storage** 
		- [Pure Storage Documentation portal](https://support.purestorage.com/bundle/m_release_notes_for_vmware_solutions/page/Solutions/VMware_Platform_Guide/Release_Notes_for_VMware_Solutions/topics/container/o_esxi_80.html)
		- 
	- **VMware Horizon**: [Compatibility Confirmation](https://interopmatrix.vmware.com/Interoperability?col=569,&row=2)

You can copy this Markdown format into your document, and it will render as a bulleted list with clickable links in Obsidian.

# Phases
Phases Summary:
- [[#Phase A - Validate Prerequisites and Planning]]
- [[#Phase B - vCenter Upgrade]]
- [[#Phase C - Host Upgrade]]
- [[#Phase D - Post-Upgrade Tasks]]
- [[#Phase E - Validate]]
## Phase A - Validate Prerequisites and Planning

- [ ] Evaluate the new 8.0 features and improvements (note the link in the doc to U1 article)
- [x] Verify Hardware Compatibility with v8.0 U2. Collect BIOS/Firmware details ✅ 2024-07-30
- [x] Review vSphere Upgrade Process ✅ 2024-07-30
- [ ] Complete Compat-Interop tab of this document re: Interoperability of VMware solutions, integrations, or Plug-ins
    - [ ] Review Skyline Finding vSphere-ESXi8checksums-KB#88877 (or KB article if Skyline is not deployed).
    - [x] This Power CLI query should return potentially affected host/vib pairs. - 📅 2024-07-30 ✅ 2024-07-30
    ```powershell
    foreach($vmhost in Get-VMHost -State:Connected|sort-object Name) {
      (Get-EsxCli -vmhost $vmhost -v2).software.vib.list.invoke() | 
      ?{ 
        ($_.name -match "vmware-perccli|vmware-storcli|emulex-esx-elxmgmt|asavp-alarming|avaya-licensing|avaya-harden|avaya-watchd|avp-alarming|avaya-tools|avaya-easg|watchd-files|native-cpld|cplduser|sas-raid_boss-cli_6.x") -OR 
        ($_.name -match "netappnasplugin" -AND $_.version -eq '1.1.2-3') 
      } | Select-Object @{N='VMhost';E={($vmhost.Name)}}, Id
    } # end foreach
    ```
- [x] Verify Hosts BIOS are at recommended level ✅ 2024-07-30
- [x] Upgrade Hosts Firmware if needed ✅ 2024-07-30
- [x] Verify hosts meet the minimum requirements (RAM, disk space, network, HBAs…etc.) ✅ 2024-07-30
- [x] Identify/Validate durable local host storage to install to ✅ 2024-07-30
- [x] Verify system requirements for new vCSA ✅ 2024-07-30
- [ ] Plan upgrade path of vCenter Server
- [x] If External PSC (< 7.0), plan the convergence to embedded (clean-up replication topology after upgrade) ✅ 2024-07-30
- [ ] If vCenter is in High Availability mode, plan upgrade accordingly
- [ ] Run lsdoctor --lscheck to find issues in the SSO domain
- [ ] Review vSphere 8.0 Update Recommended Sequence
- [ ] Plan Host Upgrade/Install Strategy/Method
- [ ] Download upgrade binaries and save in location available to hosts
- [ ] Upgrade vCenter, vSAN and ESXi License Keys to version 8
- [ ] Open Pro-active SR for plan review (Premier Only)
- [ ] Send pro-active plan-review SR to TAM and SAM
- [ ] Submit change control
- [ ] Send maintenance communication to business users and management
- [ ] Verify pure storage compatibility matrix, plugins, vasa self signed certs, etc. -pr⏫ 

## Phase B - vCenter Upgrade

- [ ] 0. Verify Prerequisites such as time sync/NTP configured on all systems
- [ ] 1. Backup PSCs, vCenter servers and database, and VUM server and database (Talk about VUM configuration, and old patches/plug-ins)
    - [ ] 1.1. Verify your VCSA File-based backups and backup schedule
    - [ ] 1.2. Document VCSA Backup Details -- Special note if using SCP this no longer exists in 7.0, you'll need to recreate as SFTP (see below).
- [ ] 2. Upgrade vCenter Server Infrastructure
    - [ ] 2.1. Prepare ESXi Hosts for vCenter Server Appliance Upgrade
    - [ ] 2.2. Upgrade vCenter Server, specifying 'this is the first vCenter Server in the Topology'
    - [ ] 2.3. (optional) Upgrade remaining vCenter Server(s), specifying 'this is a subsequent vCenter Server'
    - [ ] 2.4. (optional) Decommission any External Platform Services Controller(s)
- [ ] 3. Add new license keys and associate with assets: vCenter Server/vSAN clusters.
- [ ] 4. Verify vCenter functions/access
- [ ] 5. Verify DRS Groups that may keep PSCs/vCenters on specific hosts (as new appliances were deployed)
- [ ] 6. (optional) If NSX is present, ensure the new vCenter VM is added to the exclusion list.
- [ ] 7. Create VCSA File-Based Backup Schedule (this may already exist, unless using SCP before upgrade).

## Phase C - Host Upgrade

- [ ] 1. If necessary, migrate or power off VM on host before upgrading
- [ ] 2. Put Hosts in Maintenance Mode
- [ ] 3. Upgrade ESXi Hosts to desired version
- [ ] 4. Apply version 8 host license
- [ ] 5. Run Secure Boot Validation Script on upgraded ESXi Host
- [ ] 6. Configure Syslog on ESXi Hosts

## Phase D - Post-Upgrade Tasks

- [ ] 1. Upgrade Distributed Virtual Switch version
- [ ] 2. Upgrade VM Templates to include new VMware Tools and Hardware Version
- [ ] 3. Upgrade VMware Tools

## Phase E - Validate

- [ ] 0. Operational Verification
- [ ] 1. Verify Virtual Infrastructure Layer
- [ ] 2. Verify Operations Management Layer
- [ ] 3. Verify Cloud Management Layer
- [ ] 4. Verify Hosts health (network, storage…etc.)
- [ ] 5. Verify all Hosts and VMs can be seen in vCenter
- [ ] 6. Test vMotion
- [ ] 7. Test Backup & Restore Operations
- [ ] 8. Verify Aria Suite components (Automation, Operations, Operations for Logs, Operations for Networks)
- [ ] 9. Ask App owners to verify their applications

# **Milestones:**
Milestone Summary:
- Upgrade NSX-T to vSphere 8 compatible
- dc2viewvc - Upgrade to vSphere 8
- dc2vc - Upgrade to vSphere 8
- viewvc - Upgrade to vSphere 8
- vc - Upgrade to vSphere 8

# Roll Back Plan Details:
- Restore PSCs from backup
- Restore vCenter from backup
- Restore host from backup
- Restore VUM from backup

# Meet Notes:

# Quicknotes