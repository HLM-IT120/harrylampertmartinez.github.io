---
layout: post
title: Homelab Storage Architecture & Networking Fundamentals"
date: 2026-10-08 12:00:00 -0700
categories: [networking, storage, homelab]
---

## Homelab Storage Architecture & Networking Fundamentals

This week, my coursework progressed from data storage architecture to core networking fundamentals. Below is a breakdown of what I explored and documented this week.

### 1. Storage Management & ZFS Myths
First, I explored **Homelab Data and Storage Management** through an AI-assisted research lab, where I tackled a common hardware myth: *that running ZFS strictly requires expensive server-grade ECC RAM to prevent data corruption.* 

By cross-checking technical claims against official OpenZFS documentation, I verified that while ECC RAM is recommended for enterprise environments, standard memory is fully supported and no more risky than standard consumer file systems.

### 2. Networking Infrastructure & Local Routing
For my final assignment, I shifted to networking hardware and traffic flow, breaking down:
* **Subnets & Sub-networks:** How networks are divided by department or region to manage connectivity and security requirements.
* **Hubs vs. Switches vs. Routers:** How hubs blindly broadcast traffic across all ports into a single collision domain, whereas switches learn MAC addresses to isolate collision domains, and routers direct traffic between separate subnets using routing tables.
* **Routing Tables:** Understanding destination points, next hop IP addresses, exit interfaces, metrics/costs, and administrative distances.

### 3. Homelab Implementation
I tied this theory directly into my own homelab by deploying a **1-gigabit Ethernet switch paired with a powerline adapter** to bridge network traffic across my home's electrical wiring where running a physical Ethernet cable wasn't practical. This allows my server node to communicate at full 1Gb speed with low latency.
