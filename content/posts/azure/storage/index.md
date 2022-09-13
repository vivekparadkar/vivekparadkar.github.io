---
title: "Azure Storage"
date: 2022-09-10
menu:
  sidebar:
    name: Azure Storage
    identifier: azure-storage
    parent: azure
    weight: 20
draft: true
---

- Microsoft's cloud storage solution for modern data storage scenarios
- Offers following services:
  - Azure Blobs: Massively scalable, object store for text and binary files.
  - Azure Files: File share for on-premise and cloud.
  - Azure Queues: Reliable messaging store for applications.
  - Azure Tables: NoSQL store for schemaless data storage.
  - Azure Disks: Block Level volumes for Azure VMs.
- Each service is accessed through a **Storage Account**
- Provides following benefits:
  - Durable and Highly available
  - Secure
  - Scalable
  - Managed
  - Accessible

### Storage Account

---

- Contains all azure storage data objects.
- Provides a unique namespace accessible over HTTP or HTTPS using REST API.
- Following types of Storage accounts:
  - Standard General Purpose V2:
    - Supports all storage services: Blobs, queues, tables and file shares.
    - Recommended for most of the scenarios.
    - Supports all 6 redundancy options: LRS, GRS, RA-GRS, ZRS, GZRS and RA-GZRS.
  - Premium Block Blobs:
    - Supports block blobs and append blobs.
    - Recommended for scenarios high transaction rates, using smaller objects, low storage latency.
  - Premium File Shares
  - Premium Page Blobs
