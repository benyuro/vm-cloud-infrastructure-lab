# WEB01 Deployment

## 1. Overview

WEB01 is the first application server in the VM Cloud Infrastructure Lab.

Its responsibility is to host the web application and respond to HTTP requests received from the load balancer.

This document records the deployment, configuration, and verification of WEB01.

---

## 2. Virtual Machine Specification

| Component | Configuration |
|-----------|---------------|
| Hostname | web01 |
| Operating System | Ubuntu Server 24.04 LTS |
| RAM | 2 GB |
| vCPU | 1 |
| Virtual Disk | 20 GB |
| Storage Layout | LVM |
| Hypervisor | Oracle VirtualBox |

---

## 3. Network Configuration

WEB01 was configured with two virtual network interfaces.

| Interface | Network Mode | IP Address | Purpose |
|-----------|--------------|------------|---------|
| enp0s3 | NAT | DHCP Assigned | Internet access for updates |
| enp0s8 | Host-Only | 192.168.56.20/24 | Internal infrastructure communication |

The Host-Only interface allows communication with LB01 while remaining isolated from external users.

---

## 4. OpenSSH Configuration

OpenSSH Server was installed and enabled.

Remote administration was successfully verified from the Windows host using:

```bash
ssh benedict@192.168.56.20
```

SSH connectivity confirmed successful remote administration over the private network.

---

## 5. Nginx Installation

Nginx was installed using Ubuntu's package manager.

The service was verified to be running successfully.

HTTP requests on TCP port 80 were successfully accepted.

---

## 6. Custom Homepage

A new `index.html` file was created inside:

```
/var/www/html
```

Instead of modifying the default Debian Nginx page, a custom homepage was created to identify the backend server.

The page displays:

- Welcome to WEB01
- VM Cloud Infrastructure Lab
- Server IP
- Operational Status

---

## 7. Verification

The following tests were successfully completed:

- SSH connectivity from Windows.
- Local HTTP verification using:

```bash
curl http://localhost
```

- Remote HTTP verification from LB01 using:

```bash
curl http://192.168.56.20
```

The custom webpage was successfully served in both cases.

---

## 8. Lessons Learned

This deployment introduced several important Linux concepts:

- Linux filesystem hierarchy.
- File ownership and permissions.
- Purpose of `sudo`.
- Difference between reading a file directly and serving it through a web server.
- Nginx document root.
- HTTP verification using `curl`.
- Localhost versus remote network testing.

---

## 9. Deployment Status

**Status:** Deployed and Verified

WEB01 is operational and ready to participate in the load-balanced web tier.