---
layout: default
title: Cloud SQL
section: Reference
---

Cloud SQL instances are relational database servers you can build and manage via the [Brightbox Cloud API](/docs/reference/api/) (and therefore via our [CLI tool](http://brightbox.com/docs/guides/cli/) and [web based GUI](http://brightbox.com/docs/guides/manager/)).

Cloud SQL instances can be built in any [zone](http://brightbox.com/docs/reference/glossary/#zone) within a region. An instance's specification, such as RAM size and number of CPU cores, is defined by it's instance type.

MySQL engine version 5.5 is currently supported with more engines, such as PostgreSQL, planned for future releases.

### Access

Cloud SQL instances are made accessible by mapping [Cloud IPs](/docs/reference/cloud-ips) to them.

When an instance is first created, admin credentials are automatically generated by the API. The credentials are only displayed at create time, since the admin password is not stored by the API, so cannot be retrieved later.

A new password can be generated at any time, using the "Reset admin password" action. This generates a new admin password and updates the instance with it.

For convenience, Cloud SQL instances created from snapshots inherit the admin password from the snapshot, rather than generating a new one each time.

### Snapshots

Creating a snapshot of a Cloud SQL instance takes an instant and consistent copy of the databases stored on it. The snapshot is then copied to highly-available storage, replicated across multiple zones.

The snapshot itself is instantaneous, but the copy process can take anything from 30 seconds to several minutes depending on the size of the databases and the load on your instance.

During the copy process, the snapshot is held on the instance so the instance cannot be destroyed until the copy is completed.

New Cloud SQL instances can be built using a snapshot as a starting point, essentially cloning an instance. Instances created from snapshots inherit the admin password from the snapshot.

### Snapshotting MyISAM tables

The snapshot process depends on crash recoverable storage engines, such as [InnoDB](http://en.wikipedia.org/wiki/InnoDB). The MyISAM engine is not a crash recoverable storage engine so snapshots of MyISAM tables may result in corrupt data.

If you need to use MyISAM tables, then you can minimise the risk of corrupted tables in snapshots by flushing and locking them prior to making a snapshot. They only need to be locked for a few seconds at the start of the snapshot, and not during the copy process.

### Upgrading or downgrading a Cloud SQL Instance

To change the type of an instance, you should snapshot it and create a new instance of the desired type from the snapshot.

When downgrading, you must ensure that the snapshot data size is not larger than the capacity of the smaller instance type.

### Maintenance window

Minor version updates to the database engine are automatically applied during the maintenance window. The maintenance window is currently Sunday morning between 06:00am and 07:00am UTC. During any upgrades, service can be interrupted for short periods of time if the instance needs to be restarted.

Minor version updates are from the upstream vendor, and will usually only contain bug and security patches.

The maintenance window is currently fixed, but will be customisable in future.

### Billing

Cloud SQL instances are [billed by the hour](/pricing/#cloud_sql), starting from when it is first created until it is destroyed. Data to the instance from the Internet is billed per gigabyte at the [standard Internet data rates](/pricing/#data). Data to the instance internally from within the same region is free.

Snapshot storage is billed per gigabyte at the Image Library rate.

