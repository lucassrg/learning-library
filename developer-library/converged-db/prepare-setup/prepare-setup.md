# Prepare Setup

## Introduction
This lab will show you how to download the Oracle Resource Manager (ORM) stack zip file needed to setup the resource needed to run this workshop. This workshop requires a compute instance and a Virtual Cloud Network (VCN).

*Estimated Lab Time:* 15 minutes

### Objectives
- Download ORM stack
- Configure an existing Virtual Cloud Network (VCN)

### Prerequisites
This lab assumes you have:
- An Oracle Free Tier or Paid Cloud account
- SSH Keys

## Task 1: Download Oracle Resource Manager (ORM) stack zip file
1.  Click on the link below to download the Resource Manager zip file you need to build your environment: [converged-db-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/G0b79dLclzxjZa_txLAqGi2EXS5zmqyjIltpLeVGfSXaKCn07lIDBWSJm0UqldHF/n/natdsecurity/b/stack/o/converged-db-mkplc-freetier.zip)

2.  Save in your downloads folder.

We strongly recommend using this stack to create a self-contained/dedicated VCN with your instance(s). Skip to *Step 3* to follow our recommendations. If you would rather use an exiting VCN then proceed to the next step as indicated below to update your existing VCN with the required Egress rules.

## Task 2: Adding Security Rules to an Existing VCN   
This workshop requires a certain number of ports to be available, a requirement that can be met by using the default ORM stack execution that creates a dedicated VCN. In order to use an existing VCN the following ports should be added to Egress rules

| Port           |Description                            |
| :------------- | :------------------------------------ |
| 22             | SSH                                   |
| 1521           | Database Listener                     |
| 3000           | Node JS Application                   |
| 3001           | Node JS Application                   |
| 3003           | Node JS Application                   |
| 6080           | noVNC Remote Desktop                  |
| 7007           | Graph UI                              |
| 7101           | WebLogic Admin Console                |
| 7102           | WebLogic Admin Console                |
| 8080           | Other Apps on Docker                  |
| 9090           | ORDS                                  |
| 9502           | OAS Presentation service              |

1.  Go to *Networking >> Virtual Cloud Networks*
2.  Choose your network
3.  Under Resources, select Security Lists
4.  Click on Default Security Lists under the Create Security List button
5.  Click Add Ingress Rule button
6.  Enter the following:  
    - Source CIDR: 0.0.0.0/0
    - Destination Port Range: *Refer to above table*
7.  Click the Add Ingress Rules button

## Task 3: Setup Compute   
Using the details from the two steps above, proceed to the lab *Environment Setup* to setup your workshop environment using Oracle Resource Manager (ORM) and one of the following options:
  -  Create Stack:  *Compute + Networking*
  -  Create Stack:  *Compute only* with an existing VCN where security lists have been updated as per *Step 2* above

You may now [proceed to the next lab](#next).

## Acknowledgements

* **Author** - Rene Fontcha, Master Principal Solutions Architect, NA Technology
* **Contributors** - Kay Malcolm, Product Manager, Database Product Management
* **Last Updated By/Date** - Rene Fontcha, LiveLabs Platform Lead, NA Technology, December 2020
