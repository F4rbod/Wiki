---
title: Accessing and Working with Medicare claims data on Open OnDemand in RStudio
description: 
published: true
date: 2023-05-14T17:19:46.996Z
tags: 
editor: markdown
dateCreated: 2023-05-14T02:09:56.634Z
---

# [Guide: Accessing and Working with Medicare claims data on Open OnDemand in RStudio](https://app.tango.us/app/workflow/e84730bf-37ed-41c2-8778-31fbc762e7e5?utm_source=markdown&utm_medium=markdown&utm_campaign=workflow%20export%20links)

Welcome. To access the Medicare Claims and other datasets available from the lab, you first need to get authorization to access the Discovery Cluster. Discovery Cluster is a high-speed computational cluster that provides us with computational resources for analysis that is not possible on a personal computer.

To ask for authorization, you need to send an email to [rchelp@northeastern.edu.](mailto:rchelp@northeastern.edu)

You should copy (cc) Dr. Post in the email. His email address is [b.post@northeastern.edu](mailto:b.post@northeastern.edu).

Please use the following template for the email and complete the required parts as mentioned:

Dear RC Help:

Professor Brady Post, CCd, is the owner of /work/postresearch.

With his approval, could you please grant me READ-ONLY access to

/work/postresearch/Shared/Data\_raw/

Professor Post will also create a folder to which I am requesting WRITE access:

/work/postresearch/Shared/Researchers/\[insert your last name\]

Thanks,

\[your name\]

__Creation Date:__ May 9, 2023  
__Created By:__ Farbod  
[View most recent version on Tango.us](https://app.tango.us/app/workflow/e84730bf-37ed-41c2-8778-31fbc762e7e5?utm_source=markdown&utm_medium=markdown&utm_campaign=workflow%20export%20links)



***




### 1. [Once you are authorized to access the cluster: Go to ood.discovery.neu.edu](https://ood.discovery.neu.edu)


### 2. Type your Discovery cluster username and password. These are generally your Northeastern credentia
![Step 2 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/95921bc9-cddc-475c-86ef-6b60c7188ae6/da10a40f-917a-43b4-bde7-e0717f6280b5.png?crop=focalpoint&fit=crop&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 3. Click on Interactive Apps
![Step 3 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/23e62289-8f47-43f7-bc45-d0522f2ffad2/53f76617-145a-413a-9f83-4459669c1d1f.png?crop=focalpoint&fit=crop&fp-x=0.4757&fp-y=0.0216&fp-z=2.2561&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 4. Click on  RStudio (Rocker Container)
![Step 4 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/3ad5033b-e122-4217-8346-9d0d77243d3f/501b21bf-dfa8-4350-995d-059c4b11866f.png?crop=focalpoint&fit=crop&fp-x=0.5252&fp-y=0.5904&fp-z=1.8502&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 5. Select short from Partition:
![Step 5 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/df2b3e19-485c-40d5-aa8c-1a3cddfb4425/7480ec04-92ab-4179-abd1-26dbac7ece90.png?crop=focalpoint&fit=crop&fp-x=0.5004&fp-y=0.2813&fp-z=1.4550&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 6. Type the number of hours you need to work on the data. The maximum on the short partition is 24 hours.
![Step 6 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/7128910e-2bda-4c64-bca6-f3a5383b8ae5/2ebe65eb-3f7d-4e8e-a2a7-8151511dad03.png?crop=focalpoint&fit=crop&fp-x=0.5004&fp-y=0.2968&fp-z=1.4550&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 7. Set the number of CPU cores here.
![Step 7 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/bae08919-df5c-4bab-9227-e48839759c6c/d49a0a93-be22-43fb-a00a-7a5c7767b58d.png?crop=focalpoint&fit=crop&fp-x=0.5015&fp-y=0.3636&fp-z=1.4550&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 8. Type the amount of memory you need. You generally need 180GB to work on "all_years" data that have select variables only. If you want to import every single year of data with all the variables, you need more than 400GB. Carrier files are especially large.
![Step 8 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/bc1c882a-c98e-4cbb-946c-6b84a800f836/84c2e643-0b35-486b-a978-1fa523e77553.png?crop=focalpoint&fit=crop&fp-x=0.5004&fp-y=0.4320&fp-z=1.4550&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 9. Click on Launch
![Step 9 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/e4355322-d8eb-46a7-9050-37d7e4650b4f/f9043f7c-cf88-444e-b396-ba6d2de93fc7.png?crop=focalpoint&fit=crop&fp-x=0.5004&fp-y=0.7032&fp-z=1.4550&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 10. Wait while the cluster allocates you a node. Click on Connect to RStudio Server once it is available.
![Step 10 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/aff7258c-539d-47f1-86cc-4f4f98f9516f/89ff950c-fccc-4121-94f6-e1d0590ed505.png?crop=focalpoint&fit=crop&fp-x=0.4346&fp-y=0.3694&fp-z=1.9290&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 11. Create a new R Script, or an R Markdown file, based on your preference.
![Step 11 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/bf7b4551-7a11-4c01-bab1-d8caecc5f1e2/51b61e0e-188d-46c0-bd02-957dae28b0d6.png?crop=focalpoint&fit=crop&fp-x=0.1123&fp-y=0.0572&fp-z=2.7905&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 12. See the working directory using getwd() and set it to your folder using setwd() (replace Farbod with your name if you have a folder).
The address will be "/work/postresearch/Shared/Researchers/[insert your last name" as you only have write access to this folder. 
Once you set your working directory to the above address using setwd(), you can use commands like write.csv() to save your results. When your working directory is set to your folder, every address you enter within write.csv() will save to your folder. For example, if you want to save a dataset called "2018\_results" to your folder, you would enter

write.csv(2018\_results, "2018\_results.csv").

This will save a file called 2018\_results.csv in your folder.
![Step 12 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/ddccc776-0ef5-4a2e-863f-1bcfd7cb10a8/b4caf6f8-3300-471f-884c-1579f21d1af1.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 13. See the working directory using getwd() and set it to your folder using setwd() (replace Farbod with your name if you have a folder).
![Step 13 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/c64e91a6-0ffb-4ebb-b732-59f878011146/fec0e134-3bc8-406e-82c8-7b6c68cc1314.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


### 14. use the read_fst function to import the data. The data are saved at "/work/postresearch/Shared/Projects/Data_fst". 
Here are two examples of the data. The first one imports all the variables for the carrier file from 2018. The second one imports a restricted but useful set of variables from all years of inpatient data. You can see all the available data here:  
[https://ood.discovery.neu.edu/pun/sys/dashboard/files/fs//work/postresearch/Shared/Projects/Data\_fst](https://ood.discovery.neu.edu/pun/sys/dashboard/files/fs//work/postresearch/Shared/Projects/Data_fst)

You can also see this folder for other data and the data in other formats:

[https://ood.discovery.neu.edu/pun/sys/dashboard/files/fs//work/postresearch/Shared/Data\_raw](https://ood.discovery.neu.edu/pun/sys/dashboard/files/fs//work/postresearch/Shared/Data_raw)

Notably, if you want to use SAS or Python, the sas7bdat and CSV files for Medicare claims data are here:  
  
[https://ood.discovery.neu.edu/pun/sys/dashboard/files/fs//work/postresearch/Shared/Data\_raw/Medicare/Claims](https://ood.discovery.neu.edu/pun/sys/dashboard/files/fs//work/postresearch/Shared/Data_raw/Medicare/Claims)
![Step 14 screenshot](https://images.tango.us/workflows/e84730bf-37ed-41c2-8778-31fbc762e7e5/steps/68280a7c-e783-4e17-ad12-6a11272b181e/396ebd83-b6ae-49b6-881c-335f3bcca5b4.png?crop=focalpoint&fit=crop&fp-x=0.2723&fp-y=0.3296&fp-z=1.1964&w=1200&border=2%2CF4F2F7&border-radius=8%2C8%2C8%2C8&border-radius-inner=8%2C8%2C8%2C8&blend-align=bottom&blend-mode=normal&blend-x=0&blend-w=1200&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmstdjIucG5n)


***
Created with [Tango.us](https://tango.us?utm_source=markdown&utm_medium=markdown&utm_campaign=workflow%20export%20links)