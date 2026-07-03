# Infrastructure Design

## 1. Overview

This document defines the initial infrastructure architecture for the VM Cloud Infrastructure Lab.

The environment is designed to simulate a small production-style application infrastructure using multiple Ubuntu Server virtual machines hosted on Oracle VirtualBox.

The architecture separates the load balancing, web application, and database responsibilities across dedicated virtual machines. This separation is intended to improve infrastructure organization, provide web-tier redundancy, and demonstrate core infrastructure engineering concepts such as load balancing, network segmentation, high availability, and failure analysis.

The current architecture is a learning environment and does not claim to provide complete end-to-end high availability. Known single points of failure are identified and documented as part of the architecture review.

---

## 2. Architecture Goals

The infrastructure is designed with the following goals:

* Separate infrastructure responsibilities across dedicated servers.
* Provide redundancy at the web server tier.
* Distribute application traffic across multiple web servers.
* Isolate the database from direct user access.
* Identify and document single points of failure.
* Simulate production-style infrastructure within the available local hardware resources.
* Build an environment that can be expanded with additional high availability components in future phases.

---

## 3. Server Architecture

The initial environment consists of four virtual machines.

| Server | Hostname | Role            | Operational Design   |
| ------ | -------- | --------------- | -------------------- |
| VM1    | LB01     | Load Balancer   | Single Load Balancer |
| VM2    | WEB01    | Web Server      | Active               |
| VM3    | WEB02    | Web Server      | Active               |
| VM4    | DB01     | Database Server | Primary Database     |

### LB01 — Load Balancer

LB01 serves as the primary entry point into the application infrastructure.

The load balancer receives incoming application requests and distributes traffic between WEB01 and WEB02. Health checks will be used to determine whether a web server is available to receive traffic.

If one web server becomes unavailable, the load balancer should stop routing requests to the unhealthy server and continue sending traffic to the remaining healthy web server.

### WEB01 and WEB02 — Web Servers

WEB01 and WEB02 operate in an active-active configuration.

Both servers are available to process application requests during normal operation. The use of two web servers reduces the web tier's dependency on a single server and provides redundancy if either WEB01 or WEB02 fails.

Traffic distribution between the web servers will be managed by LB01.

### DB01 — Database Server

DB01 provides the database service required by the application.

The database server is separated from the web servers to isolate database responsibilities and reduce direct exposure to users.

WEB01 and WEB02 will communicate with DB01 when application data must be created, retrieved, or updated.

---

## 4. Traffic Flow

The expected application traffic flow is:

User → LB01 → WEB01 or WEB02 → DB01

Users connect to the application through the load balancer.

LB01 evaluates the health of the available web servers and forwards the request to a healthy backend server.

The selected web server processes the application request and communicates with DB01 when database access is required.

DB01 is not intended to receive direct application requests from users.

---

## 5. High Availability Design

The initial architecture provides redundancy at the web server tier.

WEB01 and WEB02 operate using an active-active design. Both web servers can process application traffic during normal operation.

If WEB01 fails, LB01 should detect the failure and route application traffic to WEB02.

If WEB02 fails, LB01 should continue routing application traffic to WEB01.

This design removes an individual web server as a single point of failure.

However, the initial architecture does not provide complete end-to-end high availability.

---

## 6. Identified Single Points of Failure

### 6.1 Load Balancer — LB01

LB01 is currently a single point of failure.

Although the web tier contains two active web servers, users access the application through LB01. If LB01 becomes unavailable, the application entry point is lost and users will be unable to access the application through the designed traffic path.

WEB01 and WEB02 may remain operational, but application traffic will no longer be routed to them through the load balancer.

#### Proposed Future Improvement

A second load balancer, LB02, could be introduced.

LB01 and LB02 could operate using an active-passive design with a Virtual IP address and a failover mechanism such as Keepalived.

If the active load balancer fails, the standby load balancer could take ownership of the Virtual IP address and continue receiving application traffic.

---

### 6.2 Database Server — DB01

DB01 is also a single point of failure.

Both WEB01 and WEB02 depend on DB01 for application data. If DB01 becomes unavailable, the web servers may remain accessible but database-dependent application functions may fail.

These functions may include:

* User authentication.
* Retrieving application data.
* Creating new records.
* Updating existing records.

The application may therefore become functionally unavailable even though the web servers remain online.

#### Proposed Future Improvement

A standby database server, DB02, could be introduced.

DB01 could operate as the primary database while DB02 operates as a standby database.

Database replication would be required to maintain a sufficiently current copy of application data on DB02.

If DB01 fails, a failover process could promote DB02 to the primary database role.

---

## 7. Current Architecture Limitations

The current four-VM architecture intentionally prioritizes learning objectives and available local hardware resources.

Known limitations include:

* A single load balancer.
* A single database server.
* No load balancer failover mechanism.
* No database replication.
* No automated database failover.
* No dedicated monitoring server.

These limitations are documented rather than ignored and may be addressed during future project phases or project extensions.

---

## 8. Future Architecture Improvements

Potential improvements to the infrastructure include:

* Deploying LB02 for load balancer redundancy.
* Implementing Keepalived and a Virtual IP address.
* Deploying DB02 as a standby database server.
* Configuring database replication.
* Implementing database failover procedures.
* Deploying a dedicated monitoring server.
* Introducing containerization using Docker.
* Evaluating infrastructure automation and configuration management tools.

---

## 9. Architecture Status

**Status:** Initial Design

The architecture will be reviewed and updated as the infrastructure is implemented, tested, and expanded.
