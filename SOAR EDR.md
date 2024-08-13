# SOAR EDR PROJECT

**By Adeen Mumtaz**




## 1. OVERVIEW

In this project, we will create an EDR using LimaCharlie and Tines. We will learn how to develop our own detection and response rules and create playbooks for automation.

What we will be doing:

1. Sending a Slack message and an email containing information about detecting malicious content. The detection will be made by LimaCharlie.
1. Tines will be generating a user prompt to either isolate the machine or not.
1. If the user selects YES then LimaCharlie will isolate the machine 

## 2. WORKFLOW

### Project Steps

- Create a detection in LimaCharlie-for example, hacktool detection, and then push it to Tines. Tines will then send a Slack message and email about detection.
- Slack messages and emails will contain the following information:
1) Time 
1) Computer Name
1) Source IP
1) Process
1) Command Line
1) File Path
1) Sensor ID
1) Link to detection file (if applicable)
- Tines will then prompt the user to either isolate the machine or not
- If the user selects *YES*

  then LimaCharlie should isolate the machine and send a message to the slack with isolation status and a note that “the computer <computername> has been isolated”

- If the user selects *NO* 

  then LimaCharlie will not isolate the machine and send a message to Slack with isolation status and a note that “the computer <computername> was not isolated”

### Workflow Diagram

![soar edr](https://github.com/user-attachments/assets/5b799a1b-0a8f-47ea-8ce5-2f49ccf67062)


## 3. **LimaCharlie Installation** 

<a name="page4"></a>According to the documentation, “ LimaCharlie is a SecOps Cloud Platform that aims to deliver security operations for the modern era.”  

LimaCharlie is a cybersecurity platform that provides Endpoint Detection and Response (EDR) capabilities as a service. It offers tools for security teams to monitor, detect, and respond to threats across their network. The platform is designed to be highly flexible and scalable, allowing users to integrate it into their existing security infrastructure and workflows.

Create an account on LimaCharlie and create an organization (like a project) with the name “SOAR EDR”

![image](https://github.com/user-attachments/assets/051a6d4d-fb10-433d-8aff-aab2176e414b)


### LimaCharlie Navigation and Overview

We will be using detections and automation in our project frequently.

![image](https://github.com/user-attachments/assets/6734e84c-3999-4bef-8e72-4f2da1d784a5)

<a name="page5"></a>Outputs is where we will be sending detection out to Tines.

![image](https://github.com/user-attachments/assets/ac69ab94-f2b2-4c6d-aa05-f05cc9822d46)

And we can navigate further and check out the other tabs as well.

### Sensor Installation

In LimaCharlie, "sensors" refer to software agents deployed on endpoints (e.g., desktops, laptops, servers) to monitor and collect telemetry data. These sensors play a crucial role in the platform’s ability to detect, analyze, and respond to security threats in real time.
![image](https://github.com/user-attachments/assets/b59e17c6-9ae3-42c7-8ea6-1ad5a2124e2b)
We will be using Windows 10 in our project. Download the windows sensor which will be used to integrate LimaCharlie into our environment 

![image](https://github.com/user-attachments/assets/32917e9a-b148-41e7-9271-6f573a8f9694)

We are interested in EDR and that is where we will download the sensor file for Windows.

![image](https://github.com/user-attachments/assets/c91d840e-da5f-45dd-a97f-cdd4fd1327cc)

For the sensor, we need the installation key to enroll our Windows machine over to LimaCharlie. We will create a new installation key for our machine.
![image](https://github.com/user-attachments/assets/a0716dc0-1ed1-4ef9-8dd0-8c8753b138f0)
![image](https://github.com/user-attachments/assets/a55bf7f5-5bc4-4e2e-9584-7a66067c82ab)

Copy the installation key as it will be used in the installation command for LimaCharlie.

![image](https://github.com/user-attachments/assets/65107ac7-09ff-43ff-a558-bbecc665afaf)

Use command 

*#./file.exe -i installation key*

![image](https://github.com/user-attachments/assets/140a1ef4-ec85-4f05-8f06-4b3202bbc2fd)

Head over to services to see if LimaCharlie is running on our Windows machine.

![image](https://github.com/user-attachments/assets/2810ac10-8511-48cc-8499-1beadca5596b)

### Navigating Sensor Information In LimaCharlie

Move to the sensor list under the sensor tab in LimaCharlie and locate the server/PC name.
![image](https://github.com/user-attachments/assets/c77f7878-6fe9-4185-9220-ab3c3b84c9f5)
![image](https://github.com/user-attachments/assets/afae83a0-9432-4a68-915a-346da04ad831)

We can see a collection of information, most likely everything on our server or machine can be accessed and viewed via LimaCharlie

![image](https://github.com/user-attachments/assets/a4e2fb1e-5840-4042-ae9b-f817977793a4)


The console lets us execute commands as we execute them on our local machine.

![image](https://github.com/user-attachments/assets/531b575a-4363-4ff3-b6f7-a9356e530084)

The network tab tells us about different network connections on our server.

![image](https://github.com/user-attachments/assets/d6638b28-8227-4c78-b985-0c279f016e60)

All the processes running on the system can be viewed here and we can also kill, modify, and do various stuff here as well.

![image](https://github.com/user-attachments/assets/cdb68027-232c-4f9b-a2e3-244937313f9b)

A great thing is that the complete file system can also be accessed here.

![image](https://github.com/user-attachments/assets/d9b76cba-749f-42d9-96ac-4df6496afe24)

Other things like processes, services, timelines, users, etc can also be viewed and accessed on LimaCharlie. 

## 4. CREATING DETECTION AND RESPONSE RULE

Now let’s move to our next step, which is to create a detection and response rule in LimaCharlie. 

Firstly we generate telemetry (i.e.; lazagne) so that it can be detected by LimaCharlie and then send it over to Tines for automation.

### LAZAGNE

The LaZagne project is an open-source application used to retrieve passwords stored on a local computer. LaZagne can extract passwords from various software on different operating systems, including Windows, Linux, and macOS. The tool is often used by penetration testers and security researchers to demonstrate the risks associated with password storage practices on local machines.

Download Lazagne from GitHub using the link: <https://github.com/AlessandroZ/LaZagne/releases/> 

Make sure to turn the defender off when downloading. 

Now execute it and then see what we get in LimaCharlie

![image](https://github.com/user-attachments/assets/8af5dc70-29e5-43f7-94ba-dd4169c7094c)

Let’s head to Timeline in LimaCharlie and view the events. Here’s the download event.

![image](https://github.com/user-attachments/assets/b5fd10e0-417a-45ca-ba4b-7bb3b2359430)

Next we see the NEW-PROCESS event that is generated when we executed the Lazagne.exe file. Let’s head over to the event and check out the information

![image](https://github.com/user-attachments/assets/98701dc1-44d7-4ac1-aec6-8b369fba4648)

From this information we are going to create our rule. 

### Detection And Response Rule

Head over to Detections and then D&R rules. From there click on the “New Rule” button.

![image](https://github.com/user-attachments/assets/cebe3763-344e-4d25-a41b-a6c4a8eec7e9)

(If you are new to rule creation, you can take hints from already created rules to see how a new rule can be created)

### Detection Rule
![image](https://github.com/user-attachments/assets/95847e43-ce0b-4190-8dc9-0151346f7e74)

The rule trigger if the process event is under the *New Process* or *Existing process* type and the machine is windows machine. The rule will ignore case sensitivity and then search for the file path ending with lazagne.exe, and search for ‘all’ flag at the end and also compares the hash value.

### Response
![image](https://github.com/user-attachments/assets/7df221f9-5352-42e2-9698-71eb92c631a0)
![image](https://github.com/user-attachments/assets/1c3485c2-887b-4bfc-8794-30ce107acaf2)
### Testing

We will save and test the rule. 
Grab the event from Timeline and paste it in test area provided at the end of the rule page.

![image](https://github.com/user-attachments/assets/1ef26bdd-b177-414f-a5df-aa1bca6ce9d4)
### Result

![image](https://github.com/user-attachments/assets/59ed6d7e-0dee-4aee-996f-e9d51a7ee590)
So our rule is working and providing detection when Lazagne.exe is executed. 
## 5. Forwarding Detection To Tines 

We will setup our slack and Tines in this section and then test our connection between LimaCharlie and Tines, making sure our soar is receiving detection made by LimaCharlie.
### Slack

Slack is a collaboration platform designed to streamline communication and collaboration within teams and organizations. It combines messaging, file sharing, and integration with other tools to create a unified workspace.

Create a new workspace in slack. Then create a channel *#alerts* for receiving messages of detection from Tines.

![image](https://github.com/user-attachments/assets/00f2fb33-1fb5-4307-9307-253c0b2a9946)
### Setting Up Tines For Detection

The **Tines** is a security automation tool designed to help security teams automate repetitive tasks, manage workflows, and respond to incidents efficiently. Unlike traditional SOAR platforms, Tines aims to simplify the automation process with a user-friendly, no-code interface.

In Tines workflows (called stories/playbook) are created that automatically perform tasks like detecting threats, responding to incidents, and managing alerts. By connecting with various security tools, Tines helps make the security process faster and more efficient.

Create a new story in Tines. Select webhook and copy the URL.

![image](https://github.com/user-attachments/assets/c62dc9e8-0d20-4567-a600-a7abffe82ec6)


We will establish a link between LimaCharlie and Tines using this URL. In LimaCharlie go to Outputs and then click add, we will see a list of outputs.

![image](https://github.com/user-attachments/assets/b3dd0b43-d410-4ab8-a212-18221dde4d85)


Select detections and from the list select Tines (we can also use webhooks if Tines is not present).

![image](https://github.com/user-attachments/assets/7bf16161-8da1-4b2e-b9e9-b7b7078fad86)


Paste the copied URL of webhook from Tines in Destination Host.

![image](https://github.com/user-attachments/assets/99642fc1-1f8f-423c-b213-a87ad4f1507d)


Save the output and check the connection to Tines.

![image](https://github.com/user-attachments/assets/ab759dd8-5b27-4dc3-ace5-7bedfa27ad51)

Now we know the detection is working in both LimaCharlie and Tines. Next, we have to setup slack for receiving detection messages. So now let’s create a playbook for automation.
## 6. Generating Playbook For Automation
In this section we will create a playbook in Tines using the workflow diagram we created before.
### Playbook
A **playbook** in the context of cybersecurity, especially within SOAR (Security Orchestration, Automation, and Response), is a predefined set of procedures and actions that are executed automatically in response to specific security incidents or threats. A playbook is like a detailed step-by-step guide for dealing with certain types of security problems. It tells you what to do and in what order to ensure the issue is handled correctly and efficiently.
The objective of this playbook will be to send a message to slack and also send an email containing information about detection. Tines will then generate a user prompt to either isolate the machine. If user selects *Yes,* then LimaCharlie should automatically isolate the machine.
The following image shows the complete Tines playbook for this project.  
### Linking Slack

First we need to connect Tines and slack. In slack go to more options and then select automations. Search for Tines and click add.

![image](https://github.com/user-attachments/assets/2f549166-7a52-4626-b538-ce66097dec09)


In Tines go back to the dashboard and then move to credentials. We need to add the slack API here. 

![image](https://github.com/user-attachments/assets/3c09f192-784e-4766-8f61-deb9f53b970e)


![image](https://github.com/user-attachments/assets/2b4559ba-ba6c-4aa7-b930-698f1aec1ab1)


![image](https://github.com/user-attachments/assets/6c365807-a543-42e6-954d-d7f8bc6297c7)


The link has been established between Tines and slack

![image](https://github.com/user-attachments/assets/18d7433e-012f-4a32-bb44-da1c3284acff)


Head back to the story in Tines and add the slack template. 

![image](https://github.com/user-attachments/assets/e89d61f1-e7a7-4f6f-9577-51e23807f2cf)


We want to send message to the slack, so search for “send a message’ on the right panel, this uses the “send message” template for slack and tells Tines to perform this action when automated. We want the message to be sent to our *alerts* channel in slack, so will copy the channel ID from channel details in slack and add it here.

![image](https://github.com/user-attachments/assets/3ba8d41f-2c2c-4f5b-8091-8c413ea88e6c)


Run and check if messages are being delivered to slack.

![image](https://github.com/user-attachments/assets/66351ac7-f4c8-464b-9813-47eac57da2c9)
## Linking Email

We are receiving messages in slack’s alert channel. Now, we also need to send an email. Add email from the items on left panel. Add description, recipient of email and the body of email.

![image](https://github.com/user-attachments/assets/20f5acb9-c700-4324-a8cc-7b8a036875ad)

Run to check if it is sending email. We get email in our inbox.

![image](https://github.com/user-attachments/assets/65a6e603-14b4-400f-8910-4cd8d061e9d5)
### Adding Detection Details

Add the details of detection in send message for slack and also in email body.

![image](https://github.com/user-attachments/assets/7e592918-53e8-4dda-8ead-5d1575a971f0)

![image](https://github.com/user-attachments/assets/0151a729-1cb6-41f7-88c2-704c03c1c6de)

Add details of detection in email body section.

![image](https://github.com/user-attachments/assets/ef3edd8f-1f6b-4750-9f9e-fae3037c3cc3)


![image](https://github.com/user-attachments/assets/3ad09134-2654-4004-944d-299d8836074f)
### USER Prompt

For generating a user prompt we will use page in Tines. Also add the details of detection here and ask the user to either isolate the machine or not.

![image](https://github.com/user-attachments/assets/0f49cc1c-a052-40fe-bba8-33c713461583)


For user’s decision we use trigger in Tines. If user selects no then we need to simply send a non-isolation status message.

![image](https://github.com/user-attachments/assets/7afc4b85-1bb6-44b0-9577-1c5c5d31656a)

In the rules section add the isolation status.

![image](https://github.com/user-attachments/assets/79e52cb5-5b89-4302-8421-c4bca88f7a8a)

Adding the same slack template as before while changing the URL.  
![image](https://github.com/user-attachments/assets/9e3e7fb9-195b-4592-b633-5cab3d1563d3)

We need to add LimaCharlie API to get isolation status from it. Go to credentials page again in Tines and add manually the credentials.
![image](https://github.com/user-attachments/assets/5f19b7dd-820e-4d0b-b50b-a0d5f79d7f95)


The API value can be obtained from LimaCharlie.

![image](https://github.com/user-attachments/assets/d7955bb3-1b88-4e98-8401-c053f3b7e3fd)


The API credential is added.

![image](https://github.com/user-attachments/assets/e35e8043-2c76-450c-b9bc-29e8d67b08d3)

Similarly, if user selects Yes the machine is isolated by LimaCharlie and the isolation message is sent to slack.

![image](https://github.com/user-attachments/assets/c546a665-cca2-4003-8c96-f09aed44dd24)

![image](https://github.com/user-attachments/assets/9235452e-8417-4b9a-8172-bfefa31dd43f)


The machine will be isolated and not able to connect outside. 

![image](https://github.com/user-attachments/assets/06fbb9f8-a695-40c5-9437-bf64cb2147f5)
## 7. Summary

In the project we use LimaCharlie to detect any malicious action on our endpoint and then automated the process of isolating the endpoint using Tines. Tines will also send detection messages to slack in particular channel for informing the team or dedicated person while also sending email about detection.



