# Multi-Cloud Data Transfer with AWS and GCP

**Author:** Caelan Dever  
**Email:** caelanwd@gmail.com

---

<img width="490" alt="gcpp" src="https://github.com/user-attachments/assets/2720ec47-f8ad-4d9d-9297-a1834f0ee321" />

---


## Introducing Today's Project!

In this project, I will demonstrate spreading your data and services across multiple cloud providers. Companies like this because it means: Higher reliability. I'm doing this project to learn Multi-cloud.

### Tools and concepts

Services I used were IAM, GCP, AWS. Key concepts I learnt include being comfortable working across multiple cloud platforms is becoming essential for building flexible and resilient systems.

### Project reflection

This project took me approximately 45 mins. The most challenging part was setting up the manifest file for selective storage transfer. It was most rewarding to be able to see the cross cloud storage transfer. 

---

## Setting up Data in S3

I started this project by setting up an S3 bucket. I uploaded files to the bucket.


<img width="923" alt="gcp" src="https://github.com/user-attachments/assets/6af8b6e6-3dd2-4cda-81d0-089d68140c0c" />

---

## Setting up GCP

Google Cloud Platform is Google's suite of cloud computing services and products. I set up a GCP account to set up the multi-cloud environment. Since the goal is to transfer data between AWS and Google Cloud, we need accounts on both platforms for this to work.

GCP's free tier includes their storage solution (i.e. their version of S3) called Cloud Storage. I also get credit to spend on services not covered by GCP's Free Tier across the first 90 days of my account.

<img width="928" alt="gcc" src="https://github.com/user-attachments/assets/fd1a9b09-ca8a-4160-a5ca-a4259c4a7929" />

---


## Storage Transfer

Data transfers are important for transfering data from AWS to GCP! Using multiple cloud providers for the transfer has benefits such as a source (AWS S3 bucket) and destination (GCP Cloud Storage bucket) for our data transfer.

The transfer is set up using Storage Transfer Service, which handles authentication between the two platforms, starts and stops the transfer, and makes sure data transfers correctly. We need this service because cloud providers don't offer native ways to connect to their competitors.

There are two different types of transfers you could set up Batch scheduling and Event-driven transfer. The difference is Batch scheduling means a one-time or scheduled transfer of data. It's great for migrating large datasets or periodic backup while Event-driven transfer, automatically transfers data whenever the source bucket gets a new or updated object. Event-driven transfer is ideal for real-time synchronization but comes with a few extra steps.

<img width="428" alt="gccc" src="https://github.com/user-attachments/assets/f31402a2-cdac-4687-83a4-86ee5a1238ff" />

---

## Granting GCP Access to AWS

To connect AWS and GCP, I'm using identity federation, which works by letting GCP temporarily get access to AWS resources without permanent credentials.  This is more secure than other methods because while IAM roles are permanent, federation access is temporary. AWS grants Storage Transfer Service short-lived credentials (15-60 minutes) for each session. Once those 15-60 minutes are up, Storage Transfer Service is back to having no access to AWS until it makes its next request for data!

I created a custom IAM role to securely access your S3 bucket from GCP. This role will grant GCP's Storage Transfer Service the permission to read data from the S3 bucket. 

Within the IAM role, I needed to write a custom trust policy because this trust policy tells AWS to trust requests coming from Storage Transfer in your GCP project (GCP calls accounts "projects"). The policy identifies GCS based on a subject ID, which is how AWS knows whether a request to transfer data is really coming from your GCP project's Storage Transfer, and not from someone else's project.

<img width="468" alt="gci" src="https://github.com/user-attachments/assets/ff37c40a-465f-4857-a1ba-c67ce7251e8a" />

---

## Transferring from S3 to GCS!

To set up my destination GCS bucket, I needed to set up its region, which means it is the most cost-effective option (data is only stored in one region) and works well when you know exactly where your data needs to be stored, perhaps for compliance or performance reasons. and its storage class, which means standard is for frequently accessed data with high performance and availability. 

I verified my data transfer was successful by seeing the files from your S3 bucket listed in your GCP Cloud Storage bucket!



<img width="490" alt="gcpp" src="https://github.com/user-attachments/assets/07b26ce0-4fe7-41ee-a2e2-12661b5f5731" />

---

## Transfer with a Manifest

In a project extension, I learned a manifest file is like a shopping list for your data transfer. Instead of transferring everything in a bucket, it lets you specify exactly which files to move. This is incredibly useful for large migrations where you might have thousands of files but only need to transfer a subset of them.

I verified my data transfer was successful by seeing the files from your S3 bucket listed in your GCP Cloud Storage bucket!

<img width="254" alt="mnf" src="https://github.com/user-attachments/assets/42c7c801-2a05-48c1-8e56-da4d3cfa1c4f" />

---
