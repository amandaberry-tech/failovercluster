# SQL Server Failover Cluster Homelab

This repository documents my project to build and test a SQL Server Failover Cluster Instance (FCI) in a homelab environment using Proxmox, Windows Server, and shared storage.

## 🔧 Project Overview

The primary goal of this project is to simulate a real-world SQL Server Failover Cluster deployment for learning purposes. The environment includes:

- A domain controller (Active Directory, DNS)
- Two SQL Server nodes (SQL01 and SQL02)
- Windows Server Failover Clustering (WSFC)
- Shared storage via iSCSI

## 🧠 Lessons Learned

When I first configured shared storage using the built-in **Windows iSCSI Target Server**, the cluster setup worked — but only in **graceful failover scenarios**. When I simulated a failure, the SQL Server role failed to come online on the secondary node.

After troubleshooting, I discovered that:

> 💡 **Windows Server Failover Clustering requires shared disks to support SCSI-3 Persistent Reservations (PR)** — including the ability to preempt and transfer ownership during a node failure.

The built-in iSCSI target does not fully support these operations, which caused disk reservation conflicts during failover.

## 🔄 Current Progress: Testing StarWind VSAN Free

To address this limitation, I’m currently testing [**StarWind Virtual SAN Free**](https://www.starwindsoftware.com/starwind-virtual-san-free) as an alternative iSCSI target. It claims to support:

- iSCSI protocol
- High availability clustering
- Support for Windows Failover Clustering

I am specifically testing whether it supports **full SCSI-3 Persistent Reservation functionality**, including the behavior required for ungraceful failover scenarios.

## 📚 Project Includes

- Configuration notes for:
  - Active Directory and DNS
  - SQL Server cluster nodes
  - Failover Cluster setup and validation
  - iSCSI target testing and shared disk configuration
- Troubleshooting logs
- PowerShell commands used
- Failover test results

## 🚀 Next Steps

- Complete testing with StarWind VSAN Free
- Explore SQL Server Always On Availability Groups as a comparison
- Add client connection tests to monitor failover behavior from the application side

---

This is a hands-on homelab project to help me better understand SQL Server high availability, Windows clustering, and shared storage infrastructure. I’m documenting the process to help others who might encounter similar limitations with entry-level iSCSI targets.
