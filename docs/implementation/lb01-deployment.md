# LB01 Deployment

## 1. Overview

LB01 is the load balancer server for the VM Cloud Infrastructure Lab.

The server was deployed as an Ubuntu Server virtual machine on Oracle VirtualBox and is intended to provide the primary application entry point and distribute traffic between WEB01 and WEB02.

This document records the initial deployment and post-build verification of LB01.

---

## 2. Virtual Machine Specification

| Component | Configuration |
|---|---|
| Hostname | lb01 |
| Operating System | Ubuntu Server 24.04 LTS |
| RAM | 1 GB |
| vCPU | 1 |
| Virtual Disk | 15 GB |
| Disk Allocation | Dynamically Allocated |
| Storage Layout | LVM |
| Hypervisor | Oracle VirtualBox |

---

## 3. Network Configuration

LB01 was configured with two virtual network interfaces.

| Interface | Network Mode | IP Address | Purpose |
|---|---|---|---|
| enp0s3 | NAT | DHCP assigned | Outbound internet access |
| enp0s8 | Host-Only | 192.168.56.10/24 | Private lab communication |

The NAT interface provides outbound connectivity for package downloads and system updates.

The Host-Only interface provides a stable private IP address for communication between the Windows host and the virtual infrastructure.

No default gateway was configured on the Host-Only interface. Internet-bound traffic uses the default route provided through the NAT interface.

---

## 4. Remote Administration

OpenSSH Server was installed during the Ubuntu Server installation.

Remote administration of LB01 was successfully tested from the Windows host using SSH over the Host-Only network.

The connection path is:

Windows Host → Host-Only Network → LB01

LB01 was accessed using its private IP address:

192.168.56.10

Successful SSH access confirmed that the server could be remotely administered through the private lab network.

---

## 5. Post-Build Verification

The following areas were verified after deployment:

- Hostname configuration.
- Ubuntu Server operating system version.
- Memory allocation.
- vCPU allocation.
- Virtual disk and LVM storage layout.
- NAT network interface.
- Static Host-Only network interface.
- Linux routing table.
- Outbound IP connectivity.
- DNS name resolution.
- OpenSSH service status.
- Remote SSH access from the Windows host.

The server successfully passed the initial post-build verification checks.

---

## 6. Installation Resource Observation

LB01 was initially configured with 1 GB RAM according to the infrastructure capacity plan.

During the Ubuntu Server ISO installation, the installer repeatedly failed before installation could complete.

The VM memory allocation was temporarily increased to 2 GB and the installation completed successfully.

After installation, LB01 was returned to its planned 1 GB runtime memory allocation.

This demonstrated that installation or bootstrap resource requirements may differ from steady-state runtime resource requirements.

The server will continue to be monitored as additional services are installed and configured.

---

## 7. Deployment Status

**Status:** Deployed and Verified

LB01 is operational and available for the next configuration phase.