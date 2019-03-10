---
title: How to set up Google Compute Engine for Data Science
tags:
  - Research
  - Deep Learning
  - GCE
  - GPU
---

## Introduction
I am writing this document for my future reference. I will go over how to set up Google Compute Engine for solving data science related problems.

## Steps
The process involves following basic steps:

* Creating a virtual instance 
* Install basic programs
* Set up Google Storage
* Adding SSD disks to the instance
* Migrating files and folders from Google Storage to SSD disk
* Upzip tar files
* Using GPU
* Set up for Jupyter Notebook/Lab

## Setting up Google Storage Bucket
Creating a storage bucket is straightforward. Take the Google Cloud Storage [Quickstart tutorial](https://console.cloud.google.com/marketplace/details/google-cloud-platform/cloud-storage) to walk you through it. 

* I have created a bucket called **rbiswasfc** and stored the data/images from NDSC competition there.
<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/google_bucket.PNG" alt="Google bucket">

## Adding  Persistent Disks (SSD)

* List the disks that are attached to the instance and find the disk that you want to format and mount. Run the following command

	* $sudo lsblk

<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/add_disk_lsb.PNG" alt="Run lsblk">

* Format the content of sdb

	* sudo mkfs.ext4 -m 0 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/sdb

<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/format_lsb.PNG" alt="Run lsblk">

* Make directory for mounting the disk. Here I am creating /mnt/disks/NDSC for storing the files for National Data Science Competetion (NDSC)
	* sudo mkdir -p /mnt/disks/NDSC

* Mount the disk (I have chosen 250GB SSD for NDSC)
	* sudo mount -o discard,defaults /dev/sdb /mnt/disks/NDSC

* Give reading and writing permissions
	* sudo chmod a+w /mnt/disks/NDSC

<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/mount_ssd.PNG" alt="Mount SSD">

## Transfering files from Google bucket to Persistent SSD

Now I need to transfer the project data from Google Bucket: rbiswas to my SSD
* First go the mounted directory
	* cd /mnt/disks/NDSC

* Copy and paste
	* gsutil cp gs://rbiswasfc/raja/* .

<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/mg_bucket_ssd.PNG" alt="Migration to SSD">

## Extract big tar files

It takes quite a while to extract big tar.gz files (>30GB). To have the progress bar while extracting we need pv

* Go to the root directory and Install pv
	* cd ~
	* sudo apt-get install pv
* Extract tar files using pv
	* pv mobile_image.tar.gz `| tar xzf -`
	* pv fashion_image.tar.gz `| tar xzf -`
	* pv beauty_image.tar.gz `| tar xzf -`
* Delete tar files to free up space
	* rm mobile_image.tar.gz

<img src="{{ site.url }}{{ site.baseurl }}/images/GCE/ssd_extract_rm.PNG" alt="Extract with PV">

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