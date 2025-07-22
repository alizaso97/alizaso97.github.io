<link rel="stylesheet" href="style.css">

# Projects

## 1) NCSA Tools and Technologies Overview

These are some of the key terms, tools, and technologies I learned about through my Security Analyst Internship with the NCSA from 06/2025 - 08/2025. I created this document as a guide for myself to reference during meetings, but I also want to distribute it among other HPC students and professionals for educational purposes.

### Security/Log Collection/VMDR

- [Splunk](www.splunk.com/en_us/blog/learn/what-splunk-does.html): designed to help organizations colelct, analyze, and act on machine data in real time. Splunk Enterprise is for on-premises environments, and Splunk Cloud Platform is for SaaS.
- [Qualys](https://www.devopsschool.com/blog/what-is-qualys-and-use-cases-of-qualys/): a cloud-based cybersecurity and vulnerability management platform designed to help organizations identify, prioritize, and remediate security vulnerabilities. The key features of Qualys are:
  - Vulnerability Assessment
  - Asset Inventory
  - Patch Management
  - Compliance Management
  - Web Application Scanning
  - Network Security Assessment
  - Container Security
  - File Integrity Monitoring (FIM)
  - Security Information and Event Management (SIEM) Integration
  - Cloud Security Posture Management (CSPM)
- [Elasticsearch](https://www.geeksforgeeks.org/elasticsearch/what-is-elastic-search-and-why-is-it-used/): an open-source, distributed search and analytics engine designed for handling large volumes of data with near real-time search capabilities. Part of the Elastic Stack, it stores data in JSON format, supports multi-tenancy, and offers powerful full-text search functionalities. Elasticsearch is part of the Elastic Stack, which includes other tools like Kibana for data visualization, Beats for data shipping, and Logstash for data processing.

### Authentication and Authorization

- [Kerberos](https://www.techtarget.com/searchsecurity/definition/Kerberos): a protocol for authenticating service requests between trusted hosts across an untrusted network, such as the internet. By providing a gateway between users and a network, Kerberos helps verify the identities of users and hosts, and it keeps unauthorized or malicious users out of a private network. Kerberos support is built into all major computer operating systems, including Microsoft Windows, Apple macOS, FreeBSD, Unix and Linux.
- [LDAP](https://www.okta.com/identity-101/what-is-ldap/): Lightweight Directory Access Protocol (LDAP) is a protocol that makes it possible for applications to query user information rapidly. Companies store usernames, passwords, email addresses, printer connections, and other static data within directories. LDAP is an open, vendor-neutral application protocol for accessing and maintaining that data. LDAP can also tackle authentication, so users can sign on just once and access many different files on the server.
- [Fakeroot](https://commandmasters.com/commands/fakeroot-linux/): this can be used to perform a preload trick to fake out the program UID presented to other services, then the programs run under the assumption that it's an authorized UID. Munge is used at NCSA to securely check that the user is legit.

### Configuration Management Tools

- [Ansible](https://docs.ansible.com/): an open source automation tool for software provisioning, configuration management, and software deployment. Functions by connecting via SSH to clients, so it doesn't need a client-side agent. It pushes modules to the clients, where they are executed locally on the client-side, then output is pushed back to the Ansible server. NCSA is looking to migrate to Ansible as development for the open source version of Puppet has ended. Ansible runs playbook against the host. Puppet doesn't work as well for networking; Ansible is better for networked equipment.
- [Puppet](https://www.puppet.com/): current configuration management tool at NCSA, firewall.pp makes firewall rules for Kerberos. Puppet runs and checks to see if the master server matches what Puppet thinks it should have, then it will fix it
- [Hiera](https://github.com/facebookresearch/hiera): used in Puppet to look up data values (e.g. for parameters). Data stored in Hiera can be retrieved in three different ways:
  - The `lookup()` function. The function-based lookups are private to the scope of the class. These variables are not meant to be changed aside from changing them in Hiera itself, meaning they are not designed to be overridden.
  - The depreciated `hiera()` function - the function above should be used instead.
  - Automatic class parameter lookups. With these, Hiera follows a specific order to look for which value to use. First, it follows the path described in `hiera.yaml` at the root of the Puppet directory. If it doesn't find a value in any of the paths configured in that file, then it will look for the value based on the `hiera.yaml` inside of the particular module. If that fails to produce a value, it then checks that module's `data/common.yaml`.
- [xCAT](https://xcat.org/): open-source distributed computing management software developed by IBM, used for the deployment and administration of Linux or AIX based clusters.

### Network Tools

- [nslookup](https://www.nslookup.io/): tool used to find DNS records for a domain name.
- [bro/zeek](https://docs.zeek.org/en/master/about.html#what-is-zeek): a passive, open-source network traffic analyzer. Developed at NCSA, zeek looks for unusual activity on the network and sends alerts. Used for network monitoring, network tapping, analyzing network traffic, and determining if connections should get blocked or send alerts. It was used to manually block connections (black hole routing (BHR), direct/manual blocks), but now the NCSA security team let Splunk decide if something should get autoblocked or alerted on.
- [bastion](https://www.lunavi.com/blog/whats-a-jumpbox-or-bastion-host-anyway): ssh jump host. These are the initial access points to get to security infrastructure. From there, the security staff can jump to Splunk, the security GitHub, etc.

### Compute Clusters

- [High-throughput computing (HTC)](https://en.wikipedia.org/wiki/High-throughput_computing): uses a different scheduler called [HTCondor](https://htcondor.org/) that operates in HTC environments. The University of Wisconsin's computer science program develops HT Condor. At UIUC, the Dark Energy Survey (DES) and small section of the campus computing cluster use HTCondor.
- [Advanced Computational Health Enclave (ACHE)](https://ng-documentation.readthedocs.io/en/latest/): the regulated computing environment for ePHI (electronic Personal Health Information)
- [mForge](https://mayoillinois.org/): the regulated computing environment for the Mayo Clinic at NCSA.
- [Nightingale Cluster](https://ng-documentation.readthedocs.io/en/latest/): the high-performance computing cluster environment within ACHE approved for Health Insurance Portability and Accountability Act (HIPAA) and Controlled Unclassified Information (CUI) data.

### Batch Scheduling

- [Slurm](https://slurm.schedmd.com/overview.html): an HPC-specific batch scheduler. Slurm is the scheduler that knows about the compute nodes in the cluster, and allocates resources in the cluster. This prevents users from all picking "node1" for their jobs and leaving "node24" under-utilized. Slurm allows you to better utilize resources on the cluster. You tell it what memory/CPUs/GPUs/how long job will run and it will allocate those resources then launch the job at a later date. This makes for a more efficient use of computing resources.
- [Munge](https://dun.github.io/munge/) is a process that handles security for Slurm because Slurm runs across all the compute nodes. It needs to know who is authorized to run. Slurm is restricted to the cluster, compute nodes, scheduler, and the login/head nodes. They don't run via Slurm, but end users connect there first. Then after submitting the job, Munge will securely tell the scheduler what their Unix user ID is so it can be added to permissions and make sure they have allocation/credits to run the job. Munge prevents users from running jobs as root or other cluster users. Without Munge you could fake the UID. Then the scheduler would trust it and run jobs as a root user on the cluster, which is not good!
- [Sun Grid Engine(SGE)](https://en.wikipedia.org/wiki/Oracle_Grid_Engine) was previously used at NCSA, now they upgraded to "Son of Grid Engine" that added Munge. Can keep running the same commands just with additional Munge support.

## 2) Virtual Data Center Tours (UNDER CONSTRUCTION)

Through my summer internship with the NCSA, I had the amazing opportunity to go on a three-day field trip where we toured some HPC data centers. This project showcases photos I took at the Argonne National Lab, Purdue University, and Indiana University. I wanted to create a "digital scrapbook" with some educational and technical information about these computing systems and their respective facilities.

### Argonne National Lab

### Purdue University

### Indiana University