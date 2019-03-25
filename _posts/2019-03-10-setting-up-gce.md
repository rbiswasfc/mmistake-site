---
title: How to set up Google Compute Engine
tags:
  - Research
  - Deep Learning
  - GCE
  - GCP
  - GPU
---

## Introduction
I am writing this document for my future reference. I will go over how to set up Google Compute Engine for solving data science related problems.

## Steps
The process involves following steps:

* Creating a virtual instance
* Install basic packages
* Create an image for future VMs
* Set up Google Bucket
* Adding SSD disks to the instance
* Migrating files and folders from Google Storage to SSD disk
* Upzip tar files
* Using GPU
* Set up for Jupyter Notebook/Lab

## Creating a virtual instance
Before you begin, you will need to have:

* A Gmail account linked with Google Cloud Platform (GCP)
* Enable billing for the GCP account with debit/credit card
* Upon logging into GCP, navigate to Google Compute Engine (GCE) > VM instances
* Click on create instance. For the current project, I selected the following options to create the VM instance
    * Name: dyslexia-untied
    * Region: asia-southeast1 (Singaopre)    Zone: asia-southeast1-b
    * Machine type: 4 vCPU
    * Memory (RAM): 16 GB
    * GPUs: NVIDIA Tesla P4
    * OS images: Ubuntu 16.04 LTS
    * Boot disk type: Standard persistent disk (50 GB)
    * Identity and API access: Check "Allow full access to all Cloud APIs"
    * Check "Allow HTTP traffic" and "Allow HTTPS traffic"

    These settings are illustrated in the following image:

<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/VM-GCE.PNG" alt="Google bucket">

NOTE: Once you have created the VM, and set it running, you can open up a terminal by pressing SSH button.

## Install basic packages
1. Anaconda: Anaconda is a free and open-source distribution of python and R programming languages for data science, and machine learning applications.
- Create a downloads directory, and switch into it.

    `mkdir downloads`

    `cd downloads`

    - Download and install the [latest Anaconda build](https://www.anaconda.com/distribution/#download-section) for linux:

    `wget http://repo.continuum.io/archive/Anaconda3-2018.12-Linux-x86_64.sh`

    `bash Anaconda3-2018.12-Linux-x86_64.sh`

    - And follow the onscreen instructions. The defaults should work fine, but make sure you answer yes to the last question about prepending the install location to PATH:

    `Do you wish the installer to prepend the Anaconda3 install location to PATH in your /home/[username]/.bashrc? [yes|no][no] >>> yes`

    - To make use of Anaconda, reboot VM and connect to it thereadter

    `sudo reboot`

2. Install unzip utility and pv (for checking eta)

    - You are very likely to download *zip* archives to your VMs, so get the unzip utility installed:

    `sudo apt-get install unzip`

    - For extracting big tar files with progress bar use pv. Go to the root directory and install pv

    `cd ~`

    `sudo apt-get install pv`

    - Extract tar files using pv e.g.

    ```pv mobile_image.tar.gz `| tar xzf -```

    - Delete tar files to free up space
     `rm mobile_image.tar.gz`

    <img src="{{ site.url }}{{ site.baseurl }}/images/GCE/ssd_extract_rm.PNG" alt="Extract with PV">

3. Install gcsfuse

    - Add the gcsfuse distribution URL as a package source and import its public key:

    ```export GCSFUSE_REPO=gcsfuse-`lsb_release -c -s` ```

    `echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list`

    ```curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -```

    - Update the list of packages available and install gcsfuse

    `sudo apt-get update`

    `sudo apt-get install gcsfuse`

## Create an image for future VMs

You now create a disk image of the VM you have built, you can create a new VM instance with all this software pre-installed.

- First of all, stop your VM using the VM instance console.

- Now go to Compute Engine > Images, and select CREATE IMAGE. From the dropdown menu, select  source disk as the VM on which you have installed Anaconda, unzip etc.

<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/image-disk.PNG" alt="Image Disk">

- Now , navigate again to the Compute Engine — VM instances page, select ‘Create Instance’, press ‘Change’ on the Boot disk panel, and select ‘Custom Images’. You should see the disk image you just created available for you as a boot disk. If you now create a new VM from this image, it will have all the software (and conda virtual env) you just installed already installed and ready to use.

## Set up Google Bucket

- Creating a storage bucket is straightforward. Take the Google Cloud Storage [Quickstart tutorial](https://console.cloud.google.com/marketplace/details/google-cloud-platform/cloud-storage) to walk you through it.

- I have created a bucket called **rbiswasfc** as follows:

    <img src="{{ site.url }}{{ site.baseurl }}/images/GCE/bucket.PNG" alt="Google bucket">

    Remember to choose the same location as VM instance for smooth data transfer

## Adding Persistent Disks (SSD)

- Navigate to your GCE VM instance details and click on edit. In the additional disk section, click on add new disk. Subsequently, fill up the details in the pop up window as per your needs, e.g.

    <img src="{{ site.url }}{{ site.baseurl }}/images/GCE/SSD.PNG" alt="SSD">

- Mount the disk by executing the following sequence:

    - List the disks that are attached to the instance and find the disk that you want to format and mount. Run the following command

	`sudo lsblk`

    <img src="{{ site.url }}{{ site.baseurl }}/images/GCE/add_disk_lsb.PNG" alt="Run lsblk">

    - Format the content of sdb

	`sudo mkfs.ext4 -m 0 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/sdb`

    <img src="{{ site.url }}{{ site.baseurl }}/images/GCE/format_lsb.PNG" alt="Run lsblk">

    - Make directory for mounting the disk. Here I am creating /mnt/disks/NDSC for storing my documents

    `sudo mkdir -p /mnt/disks/NDSC`

    - Mount the disk

    `sudo mount -o discard,defaults /dev/sdb /mnt/disks/NDSC`

    - Give reading and writing permissions

    `sudo chmod a+w /mnt/disks/NDSC`

    <img src="{{ site.url }}{{ site.baseurl }}/images/GCE/mount_ssd.PNG" alt="Mount SSD">

- To automate mounting the SSD disk upon restarting the VM, execute the following:

    - Create a backup of your current /etc/fstab file

    `sudo cp /etc/fstab /etc/fstab.backup`

    - Use the blkid command to find the UUID for the zonal persistent disk.

    `sudo blkid /dev/sdb`

    - Open the /etc/fstab file in a text editor and create an entry that includes the UUID. Make sure to use your correct disk name inplace of 'NDSC'

    `echo UUID=`sudo blkid -s UUID -o value /dev/sdb` /mnt/disks/NDSC ext4 discard,defaults,nofail 0 2 | sudo tee -a /etc/fstab`

    - Use the cat command to verify that your /etc/fstab entries are correct:

    `cat /etc/fstab`

## Transfering files from Google bucket to Persistent SSD

Now I need to transfer the project data from Google Bucket: rbiswas to my SSD
* First go the mounted directory
	* cd /mnt/disks/NDSC

* Copy and paste
	* gsutil cp gs://rbiswasfc/raja/* .

<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/mg_bucket_ssd.PNG" alt="Migration to SSD">



## GPU Set up
* Apply for GPU quota. May take 1-2 days to get approved
* Fire up VM with GPU
* Install CUDA: drivers for the GPU
	* sudo su
	* ```#!/bin/bash```
	* echo "Checking for CUDA and installing."
	* `if ! dpkg-query -W cuda; then`
  	* `curl -O http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
  		dpkg -i ./cuda-repo-ubuntu1604_8.0.61-1_amd64.deb`
  	* apt-get update

  	* sudo apt-get install cuda-8-0
	* fi
* Reboot
	* sudo reboot
* Verify its all working properly and GPU is recognized
	* nvidia-smi


<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/gpu-check-working.PNG" alt="GPU Check">

* Install cuDNN

## NN Model
Describe the predictive model and associated points

## Predictions
show some illustrative examples

## References
* [link](http://google.com)
* https://medium.com/google-cloud/using-a-gpu-tensorflow-on-google-cloud-platform-1a2458f42b0
* https://blog.cambridgespark.com/an-ideal-data-science-environment-on-a-google-virtual-machine-3bb40789b71b
*
