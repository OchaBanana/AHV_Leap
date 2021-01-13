.. title:: Introduction to Nutanix AHV

.. toctree::
  :maxdepth: 2
  :caption: Leap Labs
  :name: _leap_labs
  :hidden:

  onpremleap/onpremleap


.. _getting_started:

###############
Getting Started
###############

Legacy disaster recovery (DR) configurations use protection domains (PDs) and third-party integrations to protect your applications. These DR configurations replicate data between on-prem Nutanix clusters. Protection domains provide limited flexibility in terms of supporting complex operations (for example, VM boot order, network mapping). With protection domains, you have to perform manual tasks to protect new guest VMs as and when your application scales up.

Leap offers an entity-centric automated approach to protect and recover applications. It uses categories to group the guest VMs and automate the protection of the guest VMs as the application scales. Application recovery is more flexible with network mappings, an enforceable VM start sequence, and inter-stage delays. Application recovery can also be validated and tested without affecting your production workloads. Asynchronous, NearSync, and Synchronous replication schedules ensure that an application and its configuration details synchronize to one or more recovery locations for a smoother recovery.

What's New
==========

- Workshop updated for the following software versions:
    - AOS 5.18 & PC 2020.11


Agenda
======

- Introductions
- Leap Overview
- Protection Policies
- Recover Plan
- Failover
- Failback

Introductions
=============

- Name
- Familiarity with Nutanix

Initial Setup
=============

- Take note of the *Passwords* being used.
- Log into Nutanix X-Labs VPN (connection info below)


Lab Access Methods
==================

VPN option: Pulse Secure VPN Client
-----------------------------------

1. If client already installed skip to step 5
2. To download the client, login to https://xlv-uswest1.nutanix.com or https://xlv-useast1.nutanix.com using the supplied user credentials
  - Direct Download\
     - `Windows 32 bit <https://noc.rmutp.ac.th/wp-content/uploads/32bitinstaller.msi>`_
     - `Windows 64 bit <https://noc.rmutp.ac.th/wp-content/uploads/64bitinstaller.msi>`_
     - `Mac OS <https://noc.rmutp.ac.th/wp-content/uploads/macinstaller.dmg>`_
3. Download and install client
4. Logout of the Web UI
5. Open client and **ADD** a connection with the following details:

For PHX:

  - **Type** - Policy Secure (UAC) or Connection Server
  - **Name** - X-Labs - PHX
  - **Server URL** - xlv-uswest1.nutanix.com

6. Once setup, login with the supplied credentials `here <https://docs.google.com/spreadsheets/d/1QmYpmG1lo_3bo3iYONy-uPzIQGvnm4lHf1lx8URXj7Y/edit?usp=sharing>`_

Cluster Credentials
===================

.. list-table::
   :widths: 20 40 40
   :header-rows: 1
   * - 
     - Primary-Stie
     - Recovery-Site
   * - Cluster Name
     - Primary-Cluster
     - Recovery-Cluster
   * - PC IP
     - 10.55.21.39
     - 10.55.39.39
   * - PC Username
     - admin
     - admin
   * - PC Password
     - nx2Tech650!
     - nx2Tech656! 

.. note::

  The *<Cluster Password>* is unique to each cluster and will be provided by the leader of the Workshop.


Throughout the Workshop there are multiple instances where you will need to substitute *XYZ* with the correct octet for your subnet, for example:

.. list-table::
   :widths: 25 75
   :header-rows: 1
   * - IP Address
     - Description
   * - 10.55.\ *XYZ*\ .37
     - Nutanix Cluster Virtual IP
   * - 10.55.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.55.\ *XYZ*\ .40
     - **DC** VM IP, NTNXLAB.local Domain Controller

Each cluster is configured with 2 VLANs which can be used for VMs:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1
  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.55.\ *XYZ*\ .1/25
    - 0
    - 10.55.\ *XYZ*\ .50-10.55.\ *XYZ*\ .124
  * - Secondary
    - 10.55.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.55.\ *XYZ*\ .132-10.55.\ *XYZ*\ .253

.. list-table::
   :widths: 25 35 40
   :header-rows: 1
   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

Each cluster has a dedicated domain controller VM, **DC**, responsible for providing AD services for the **NTNXLAB.local** domain. The domain is populated with the following Users and Groups:

.. list-table::
   :widths: 25 35 40
   :header-rows: 1
   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

Nutanix Version Info
====================

- **AHV Version** - AHV 20190916.294
- **AOS Version** - 5.18.2
- **PC Version** - 2020.11
