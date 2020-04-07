# Freshservice
Freshservice is a cloud-based IT Help Desk and service management solution that enables organizations to simplify their IT operations. The solution offers features that include a ticketing system, self-service portal and knowledge-base. 

---------

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

---------

**Freshservice integration in action here**
<br>
[![Freshservice integration in action here](https://img.youtube.com/vi/jRdzZ2uUx0c/0.jpg)](https://youtu.be/jRdzZ2uUx0c?t=885)

# Pre-Requisites
* [Freshservice](https://freshservice.com/) - You can use a trial version if you don't have a sandbox environment
* xMatters account - If you don't have one, [get one](https://www.xmatters.com/signup)!

# Files
* [FreshserviceITSM.zip](FreshserviceITSM.zip) - Workflow containing all the communications and steps needed

# How it works
xMatters is using Freshservices APIs (V2) to add value with:
  1) Creating Freshservice Incidents (separate documentation can be found [here](https://github.com/dtopham802/xm-labs-step-Create-Freshservice-Incident))
  2) Finding and assigning the correct on-call resource to an incident
  3) Gathering additional on-call resources to help resolve the incident (i.e. notifing, push people into a conference bridge, push people to a chat-ops channel (Slack/MS Teams)
  4) Informing business stakeholders with an FYI notification
  5) Providing an audit trail of all actions that were taken back into Freshservice Incident (i.e device delivery/response/comments of notifications)

Since these steps are already built, you now have a drag and droppable environment to enhance the flows and build your own without even touching any code!

# Installation

## xMatters set up 
<br>
If you want to follow along with a video instead showing how to install, you can follow it here: www.youtube.com/watch?v=jRdzZ2uUx0c
<br>
<br>

1) Download the [FreshserviceITSM.zip](FreshserviceITSM.zip) file and import it into your xMatters instance. Steps to import a Workflow: [Here]( https://help.xmatters.com/ondemand/xmodwelcome/workflows/manage-workflows.htm#Import)

2) Click on your new Freshservice Workflow, Click "FLOWS", Click "Freshservice Alert"

3) After being presented with the canvas (see more about Flow Designer 101 [Here]( https://help.xmatters.com/ondemand/xmodwelcome/flowdesigner/laying-out-flow-designer.htm)), double-click on the "Freshservice - Assignment Alert - Freshservice inbound" trigger which should be at the very top of the canvas.

<kbd>
  <img src="media/Freshservice Inbound.png" width="400">
</kbd>

4) **NOTE** When you imported the Workflow, it also imported 4 steps under the "Custom" Section

   a) Freshservice - Create Incident (for more documentation on this step go [HERE](https://github.com/dtopham802/xm-labs-step-Create-Freshservice-Incident))
   
   b) Freshservice - Create Note
   
   c) Freshservice - Parse Note
   
   d) Freshservice - Update Incident
   
<kbd>
  <img src="media/Freshservice Custom Steps1.png" width="400">
</kbd>

**ALSO NOTE** This also imported a HTTP Trigger under the "TRIGGERS" Section in Flow Designer that parses payloads that come in from Freshservice.

5) In the Authenticating User Dropdown, click the API user (which should have the **REST Web Services User** role) you want to use. Click "Copy" to copy the URL it generates and save it for later. Reference General Notes section on how to create API user in xMatters.


<kbd>
  <img src="media/API User webhook.png" width="500">
</kbd>


6) Back on the canvas, double click on "Freshservice - Update Ticket" Step that is attached to the "Acknowledge response option. You will need to modify the Agent Email Input to match with the Freshservice Agent. Typically the work email is attached to the Agent in Freshservice. The example used here is the "respondedTo.by.targetName" which is the "Username" in xMatters which is typically first initial with last name (i.e. bsmith for Bob Smith). If your username in xMatters is not the same as the beginning of your work email in Freshservice, please change it in your xMatters profile (please reference screenshots for this). If they match, then just type your domain of your email where it says "@domain.com".


<kbd>
  <img src="media/Update Freshservice Step FD.png" width="500">
</kbd>


<kbd>
  <img src="media/Agent Email Update Step.png" width="500">
</kbd>


<kbd>
  <img src="media/Profile location.png" width="500">
</kbd>


<kbd>
  <img src="media/Change Username.png" width="500">
</kbd>


7) On the top right of the canvas, click on "Components", then click on "Endpoints", then "FreshService. Change the "Base URL" to whatever your domain is. Make sure the "Endpoint Type" is Basic. Put the Username and Password in for the Freshservice API user so xMatters can go back to Freshservice and evoke API calls. Double-check that the "Preemptive" box is checked.


<kbd>
  <img src="media/Enpoint location.png" width="500">
</kbd>

<kbd>
  <img src="media/Freshservice Endpoint.png" width="500">
</kbd>


8) Click the "x" on the top right of the canvas to get back to the other flows in the Workflow

9) Click on "Freshservice - Engage with xMatters" and repeat steps 3 & 5 with the "Freshservice - Engage with xMatters" Step on the very left side of the canvas.


<kbd>
  <img src="media/Freshservice Engage with xMatters.png" width="500">
</kbd>


10) Click on "Freshservice - Inform with xMatters" and repeat steps 3 & 5 with the "Freshservice - Inform with xMatters" Step on the very left side of the canvas


<kbd>
  <img src="media/Freshservice Inform with xMatters.png" width="500">
</kbd>


11) Before closing out of the Inform with xMatters canvas, double-click on the "Inform with xMatters Event" Step and type in your recipients you want to notify with a FYI notification (i.e Executive Group, FYI Group). This will be triggered when a public note is made on your incident


<kbd>
  <img src="media/Executives recipients FYI.png" width="500">
</kbd>


## Freshservice setup

1) Click "Admin" on the bottom left, Click "Form Fields". Add a boolean at the bottom of the list and label it "Engage with xMatters and click "Save". This box is to trigger and engage additional on-call resources to help work on the incident (i.e invite to Slack channel/MS Teams, spin up a Zoom bridge and invite people, etc.)


<kbd>
  <img src="media/Admin form fields.png" width="500">
</kbd>


<kbd>
  <img src="media/Ticket Engage with xMatters.png" width="500">
</kbd>


2) Click "Admin" on the bottom left, Click "Workflow Automator" under the Helpdesk Productivity section.


<kbd>
  <img src="media/Workflow Automator.png" width="500">
</kbd>


3) Click "New Automator" on the top right of the screen. Then click Ticket and give it a name (i.e. "xMatters Trigger") and a Description (i.e. "Will trigger a xMatters webhook when a high priority incident is raised")
4) Drag and drop the appropriate steps onto the automator to have it look like this:


<kbd>
  <img src="media/Workflow Automator xMatters Trigger.png" width="500">
</kbd>


**NOTE** You can change the Priority to what you want. In my example, I only have Urgent and High Priority Tickets.

5) When you get to "Trigger Webhook" Section, copy and paste the xMatters URL you got from step 5 in the xMatters set up into the "Callback URL" section.


<kbd>
  <img src="media/Webhook section Freshservice.png" width="500">
</kbd>


6) Scroll down to the Content Section and check all the boxes available and click "Done". Click Activate on the top right of the screen, then done.

7) Repeat steps 3-6 using the screenshot example below, and label it "Engage with xMatters". This time, paste the xMatters URL you got in step 7 (xMatters Setup) in the "Callback URL" section when setting up the Webhook.


<kbd>
  <img src="media/Engage checkbox automator.png" width="500">
</kbd>


**NOTE** This is making it so when that checkbox is checked in a ticket, it will trigger a xMatters event and any additional steps you have made in the flow. For now, it is a simple notification with the ticket information.

8) Repeat steps 3-6 using the screenshot example below, and label it "Inform with xMatters". This time, paste the xMatters URL you got in step 8 (xMatters Setup) in the "Callback URL" section when setting up the Webhook.


<kbd>
  <img src="media/Inform checkbox automator.png" width="500">
</kbd>


**NOTE** This will make it so you can add a public note in a Freshservice incident and will trigger a formatted notification with that information out.

## General Notes
1) Under the forms section in the Workflow, please make sure your API user has "Sender Permissions" to execute what it needs to. To find "Sender Permissions", click on "Web Services" dropdown, click on "Sender Permssions", then add your API user into the list.


<kbd>
  <img src="media/Sender Permission location.png" width="500">
</kbd>


<kbd>
  <img src="media/List of sender permission.png" width="500">
</kbd>


# Testing
1) **Ticket Assignment** Create a new Incident in Freshservice with the right priority to make sure it triggers the "Workflow Automator". Additionally, make sure there is a Group Assignment in the ticket. (**NOTE** The user that responds from the alert needs to be a part of the group in Freshservice otherwise it will error out). This will notify the on-call resource in xMatters for that group (**If you have a "Database Group" in Freshservice, you need to add it to xMatters along with the same member(s)).** That member hits accept to take ownership of the Freshservice Incident. xMatters should update the Agent Name in the incident, change the status to "In Progress", and feed in all the device deliverys, comments, and event ID into the Notes of the Incident. 


<kbd>
  <img src="media/Update Freshservice Ticket.png" width="500">
</kbd>


2) **Engage with xMatters**: This will allow you to engage multiple resources onto the incident without leaving the comfort of your own Freshservice. After following the set up above, you will be able to add Freshservice "Tags" into the ticket. For example, if you wanted to engage the "Software Development" group, you would add the "Software Development" tag to your Incident, then check the "Engage with xMatters" box in your incident to trigger the notification to the on-call resource. Keep in mind, this group needs to exist in xMatters as it is targeting that group/on-call resource. Anyone who response with Acknowledge or adds a comment, xMatters will feed that information back into the Incident for visibility/post-mortem


<kbd>
  <img src="media/Freshservice Checkbox Engage and Tag.png" width="500">
</kbd>


<kbd>
  <img src="media/Engage record back to Freshservice.png" width="500">
</kbd>


3) **Inform with xMatters**: This will allow you to keep people up to date with an FYI notification of what is going on with the incident. All you need to do here is add a public comment to the incident and xMatters will take care of the rest.


<kbd>
  <img src="media/Public Freshservice Note for FYI.png" width="500">
</kbd>


# Troubleshooting
 If it doesn't work, you can reference the "Activity Stream" of the integration. Please reference screenshot below to find the Activity Stream and what it should look like. You can look through the logs to see what the error is.


<kbd>
  <img src="media/Flow Designer Activity.png" width="500">
</kbd>

