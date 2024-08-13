3

**SOAR EDR PROJECT**

**By Adeen Mumtaz**

**Table of Contents**

|**1.	OVERVIEW.........................................................................................................................**|3|
| :- | -: |
|<p>**2.	WORKFLOW.......................................................................................................................**</p><p>Project Steps...................................................................................................................................</p><p>Workflow Diagram….......................................................................................................................</p>|<p>**3** </p><p>3</p><p>4</p>|
|<p>**3.	LIMACHARLIE INSTALLATION............................................................................................**</p><p>LimaCharlie Navigation and Overview...........................................................................................</p><p>Sensor Installation ………………………………........................................................................................</p><p>Navigating Sensor Information In LimaCharlie ..............................................................................</p>|<p>**4**</p><p>5</p><p>5</p><p>8</p>|
|<p>**4.	CREATING DETECTION AND RESPONSE RULE ....................................................................**</p><p>Lazagne…………………………………………..................................................................................................</p><p>Detection And Response Rule …………...............................................................................................</p>|<p>**12**</p><p>12</p><p>14</p>|
|<p>**5. 	FORWARDING DETECTION TO TINES ……………………...........................................................**</p><p>Slack ..................................................................................................................................................</p><p>Setting Up Tines For Detection ….....................................................................................................</p>|<p>**16**</p><p>16</p><p>17</p>|
|<p>**6.	GENERATING PLAYBOOK FOR AUTOMATION ....................................................................**</p><p>Playbook ……………………………………....................................................................................................</p><p>Linking Slack ………………………………....................................................................................................</p><p>Linking Email ……………………………………..............................................................................................</p><p>Adding Detection Details ……………………...........................................................................................</p><p>User Prompt ………………………………....................................................................................................</p>|<p>**23**</p><p>23</p><p>24</p><p>29</p><p>30</p><p>32</p>|
|**7. 	SUMMARY …........................................................................................................................**|**35**|

1. **OVERVIEW**

In this project, we will create an EDR using LimaCharlie and Tines. We will learn how to develop our own detection and response rules and create playbooks for automation.

What we will be doing:

1. Sending a Slack message and an email containing information about detecting malicious content. The detection will be made by LimaCharlie.
1. Tines will be generating a user prompt to either isolate the machine or not.
1. If the user selects YES then LimaCharlie will isolate the machine 

1. **WORKFLOW**

**Project Steps**

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

**Workflow Diagram**

![C:\Users\Adeen\Downloads\soar.drawio.png](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.001.png)

<a name="page3"></a>*Workflow Diagram*

1. **LimaCharlie Installation** 

<a name="page4"></a>According to the documentation, “ LimaCharlie is a SecOps Cloud Platform that aims to deliver security operations for the modern era.”  

LimaCharlie is a cybersecurity platform that provides Endpoint Detection and Response (EDR) capabilities as a service. It offers tools for security teams to monitor, detect, and respond to threats across their network. The platform is designed to be highly flexible and scalable, allowing users to integrate it into their existing security infrastructure and workflows.

Create an account on LimaCharlie and create an organization (like a project) with the name “SOAR EDR”

![ref1]

*LimaCharlie Installation* 

**LimaCharlie Navigation and Overview**

We will be using detections and automation in our project frequently.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.003.png)

<a name="page5"></a>Outputs is where we will be sending detection out to Tines.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.004.png)

And we can navigate further and check out the other tabs as well.

**Sensor Installation**

In LimaCharlie, "sensors" refer to software agents deployed on endpoints (e.g., desktops, laptops, servers) to monitor and collect telemetry data. These sensors play a crucial role in the platform’s ability to detect, analyze, and respond to security threats in real time. ![ref1]

We will be using Windows 10 in our project. Download the windows sensor which will be used to integrate LimaCharlie into our environment 

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.005.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.006.png)

We are interested in EDR and that is where we will download the sensor file for Windows.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.007.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.008.png)

For the sensor, we need the installation key to enroll our Windows machine over to LimaCharlie. We will create a new installation key for our machine.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.009.png)

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.010.png)

Copy the installation key as it will be used in the installation command for LimaCharlie.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.011.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.012.png)

Use command 

*#./file.exe -i installation key*

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.013.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.014.png)

Head over to services to see if LimaCharlie is running on our Windows machine.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.015.png)

**Navigating Sensor Information In LimaCharlie**

Move to the sensor list under the sensor tab in LimaCharlie and locate the server/PC name.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.016.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.017.png)

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.018.png)

We can see a collection of information, most likely everything on our server or machine can be accessed and viewed via LimaCharlie

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.019.png)

The console lets us execute commands as we execute them on our local machine.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.020.png)

The network tab tells us about different network connections on our server.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.021.png)

All the processes running on the system can be viewed here and we can also kill, modify, and do various stuff here as well.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.022.png)

A great thing is that the complete file system can also be accessed here.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.023.png)

Other things like processes, services, timelines, users, etc can also be viewed and accessed on LimaCharlie. 

1. **CREATING DETECTION AND RESPONSE RULE**

Now let’s move to our next step, which is to create a detection and response rule in LimaCharlie. 

Firstly we generate telemetry (i.e.; lazagne) so that it can be detected by LimaCharlie and then send it over to Tines for automation.

**LAZAGNE**

The LaZagne project is an open-source application used to retrieve passwords stored on a local computer. LaZagne can extract passwords from various software on different operating systems, including Windows, Linux, and macOS. The tool is often used by penetration testers and security researchers to demonstrate the risks associated with password storage practices on local machines.

Download Lazagne from GitHub using the link: <https://github.com/AlessandroZ/LaZagne/releases/> 

Make sure to turn the defender off when downloading. 

Now execute it and then see what we get in LimaCharlie

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.024.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.025.png) ![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.026.png)

Let’s head to Timeline in LimaCharlie and view the events.

Here’s the download event.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.027.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.028.png)

Next we see the NEW-PROCESS event that is generated when we executed the Lazagne.exe file. 

Let’s head over to the event and check out the information

![ref2]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.030.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.031.png)

From this information we are going to create our rule. 

**Detection And Response Rule**

Head over to Detections and then D&R rules. From there click on the “New Rule” button.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.032.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.033.png)

` `(If you are new to rule creation, you can take hints from already created rules to see how a new rule can be created)

**Detection Rule![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.034.png)**

The rule trigger if the process event is under the *New Process* or *Existing process* type and the machine is windows machine.

The rule will ignore case sensitivity and then search for the file path ending with lazagne.exe, and search for ‘all’ flag at the end and also compares the hash value.

**Response**

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.035.png)

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.036.png)

**Testing**

We will save and test the rule. 

Grab the event from Timeline and paste it in test area provided at the end of the rule page.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.037.png)![ref3]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.039.png)

**Result**

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.040.png)

So our rule is working and providing detection when Lazagne.exe is executed. 

1. **Forwarding Detection To Tines** 

We will setup our slack and Tines in this section and then test our connection between LimaCharlie and Tines, making sure our soar is receiving detection made by LimaCharlie.

**Slack**

Slack is a collaboration platform designed to streamline communication and collaboration within teams and organizations. It combines messaging, file sharing, and integration with other tools to create a unified workspace.

Create a new workspace in slack. Then create a channel *#alerts* for receiving messages of detection from Tines.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.041.png)

**Setting Up Tines For Detection**

The **Tines** is a security automation tool designed to help security teams automate repetitive tasks, manage workflows, and respond to incidents efficiently. Unlike traditional SOAR platforms, Tines aims to simplify the automation process with a user-friendly, no-code interface.

In Tines workflows (called stories/playbook) are created that automatically perform tasks like detecting threats, responding to incidents, and managing alerts. By connecting with various security tools, Tines helps make the security process faster and more efficient.

Create a new story in Tines. Select webhook and copy the URL.

![ref4]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.043.png)

We will establish a link between LimaCharlie and Tines using this URL. 

In LimaCharlie go to Outputs and then click add, we will see a list of outputs.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.044.png)

Select detections and from the list select Tines (we can also use webhooks if Tines is not present).

![ref4]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.045.png)

Paste the copied URL of webhook from Tines in Destination Host.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.046.png)

Save the output and check the connection to Tines.

![ref5]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.048.png)

Generate the event again by executing lazagne. We get a sample output for Tines.

![ref6]![ref6]![ref6]![ref6]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.050.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.051.png)

Head over to Tines to check for event detection.

From webhook click events and then select the most recent event.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.052.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.053.png)![ref6]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.054.png)

We get the same exact detection on Tines.

![ref7]![ref7]![ref7]![ref7]![ref7]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.056.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.057.png)

Now we know the detection is working in both LimaCharlie and Tines. Next, we have to setup slack for receiving detection messages.

So now let’s create a playbook for automation.

1. **Generating Playbook For Automation** 

In this section we will create a playbook in Tines using the workflow diagram we created before.

**Playbook**

A **playbook** in the context of cybersecurity, especially within SOAR (Security Orchestration, Automation, and Response), is a predefined set of procedures and actions that are executed automatically in response to specific security incidents or threats. A playbook is like a detailed step-by-step guide for dealing with certain types of security problems. It tells you what to do and in what order to ensure the issue is handled correctly and efficiently.

The objective of this playbook will be to send a message to slack and also send an email containing information about detection. Tines will then generate a user prompt to either isolate the machine. If user selects *Yes,* then LimaCharlie should automatically isolate the machine.

The following image shows the complete Tines playbook for this project.  

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.058.png)

**Linking Slack**

First we need to connect Tines and slack.

In slack go to more options and then select automations. Search for Tines and click add.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.059.png)

In Tines go back to the dashboard and then move to credentials. We need to add the slack API here. 

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.060.png)

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.061.png)

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.062.png)

The link has been established between Tines and slack

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.063.png)

Head back to the story in Tines and add the slack template. 

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.064.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.065.png)

We want to send message to the slack, so search for “send a message’ on the right panel, this uses the “send message” template for slack and tells Tines to perform this action when automated.

We want the message to be sent to our *alerts* channel in slack, so will copy the channel ID from channel details in slack and add it here.

![ref5]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.066.png)

Run and check if messages are being delivered to slack.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.067.png)

**Linking Email**

We are receiving messages in slack’s alert channel. Now, we also need to send an email. Add email from the items on left panel. Add description, recipient of email and the body of email.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.068.png)![ref5]![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.069.png)

Run to check if it is sending email.

We get email in our inbox.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.070.png)

**Adding Detection Details**

Add the details of detection in send message for slack and also in email body.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.071.png)

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.072.png)

Add details of detection in email body section.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.073.png)

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.074.png)

**USER Prompt**

For generating a user prompt we will use page in Tines. Also add the details of detection here and ask the user to either isolate the machine or not.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.075.png)

For user’s decision we use trigger in Tines. If user selects no then we need to simply send a non-isolation status message.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.076.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.077.png)

In the rules section add the isolation status.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.078.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.079.png)

Adding the same slack template as before while changing the URL.  

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.080.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.081.png)![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.082.png)

We need to add LimaCharlie API to get isolation status from it. Go to credentials page again in Tines and add manually the credentials.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.083.png)

The API value can be obtained from LimaCharlie.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.084.png) 

The API credential is added.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.085.png)

Similarly, if user selects Yes the machine is isolated by LimaCharlie and the isolation message is sent to slack.

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.086.png)

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.087.png)

The machine will be isolated and not able to connect outside. 

![](Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.088.png)

1. **Summary**

In the project we use LimaCharlie to detect any malicious action on our endpoint and then automated the process of isolating the endpoint using Tines. Tines will also send detection messages to slack in particular channel for informing the team or dedicated person while also sending email about detection.




[ref1]: Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.002.png
[ref2]: Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.029.png
[ref3]: Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.038.png
[ref4]: Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.042.png
[ref5]: Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.047.png
[ref6]: Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.049.png
[ref7]: Aspose.Words.a42b4ec1-1d2f-4866-b776-fc26e2b59485.055.png
