# Freshservice
Freshservice is a cloud-based IT Help Desk and service management solution that enables organizations to simplify their IT operations. The solution offers features that include a ticketing system, self-service portal and knowledge-base. 

[![Freshservice Integration](https://img.youtube.com/vi/Dj2sEaZzXi0/0.jpg)](https://www.youtube.com/watch?v=Dj2sEaZzXi0&feature=youtu.be)

# Pre-Requisites
* [Freshservice](https://freshservice.com/) - You can use a trial version if you don't have a sandbox environment
* xMatters account - If you don't have one, [get one](https://www.xmatters.com/signup)!

# Files
* [FreshserviceITSM.zip](FreshserviceITSM.zip) - Workflow containing all the communications and steps needed
* Video file of showing the integration

# How it works
xMatters is using Freshservices APIs (V2) to add value with:
  1) Creating Freshservice Incidents
  2) Finding and assigning the correct on-call resource to an incident
  3) Gathering additional on-call resources to help resolve the incident (i.e. notifing, push people into a conference bridge, push people to a chat-ops channel (Slack/MS Teams)
  4) Informing business stakeholders with an FYI notification
  5) Providing an audit trail of all actions that were taken back into Freshservice Incident (i.e device delivery/response/comments of notifications)

Since these steps are already built, you now have a drag and droppable environment to enhance the flows and build your own without even touching any code!

# Installation

## xMatters set up 

1) Download the [FreshserviceITSM.zip](FreshserviceITSM.zip) file and import it into your xMatters instance. Steps to import a Workflow: [Here]( https://help.xmatters.com/ondemand/xmodwelcome/workflows/manage-workflows.htm#Import)

2) Click on your new Freshservice Workflow, Click "FLOWS", Click "Freshservice Alert"

3) After being presented with the canvas (see more about Flow Designer 101 [Here]( https://help.xmatters.com/ondemand/xmodwelcome/flowdesigner/laying-out-flow-designer.htm)), double-click on the "Freshservice - Assignment Alert - Freshservice inbound" trigger which should be at the very top of the canvas.

<kbd>
  <img src="media/Freshservice Inbound.png" width="200" height="400">
</kbd>

4) **NOTE** When you imported the Workflow, it also imported 4 steps under the "Custom" Section

   a) Freshservice - Create Incident (for more documentation on this step go HERE)
   
   b) Freshservice - Create Note
   
   c) Freshservice - Parse Note
   
   d) Freshservice - Update Incident
   
<kbd>
  <img src="media/Freshservice Custom Steps1.png" width="200" height="400">
</kbd>

**ALSO NOTE** This also imported a HTTP Trigger under the "TRIGGERS" Section in Flow Designer that parses payloads that come in from Freshservice.

5) In the Authenticating User Dropdown, click the API user (which should have the **REST Web Services User** role) you want to use. Click "Copy" to copy the URL it generates and save it for later. Reference General Notes section on how to create API user in xMatters.

<kbd>
  <img src="media/API User webhook.png" width="400" height="200">
</kbd>

6) On the top right of the canvas, click on "Components", then click on "Endpoints", then "FreshService. Change the "Base URL" to whatever your domain is. Make sure the "Endpoint Type" is Basic. Put the Username and Password in for the Freshservice API user so xMatters can go back to Freshservice and evoke API calls. Double-check that the "Preemptive" box is checked.

<kbd>
  <img src="media/Enpoint location.png" width="300" height="200">
</kbd>

<kbd>
  <img src="media/Freshservice Endpoint.png" width="300" height="200">
</kbd>

7) Click the "x" on the top right of the canvas to get back to the other flows in the Workflow

8) Click on "Freshservice - Engage with xMatters" and repeat steps 3 & 5 with the "Freshservice - Engage with xMatters" Step on the very left side of the canvas.

<kbd>
  <img src="media/Freshservice Engage with xMatters.png" width="300" height="200">
</kbd>

9) Click on "Freshservice - Inform with xMatters" and repeat steps 3 & 5 with the "Freshservice - Inform with xMatters" Step on the very left side of the canvas

<kbd>
  <img src="media/Freshservice Inform with xMatters.png" width="300" height="200">
</kbd>

10) Before closing out of the Inform with xMatters canvas, double-click on the "Inform with xMatters Event" Step and type in your recipients you want to notify with a FYI notification (i.e Executive Group, FYI Group). This will be triggered when a public note is made on your incident

<kbd>
  <img src="media/Executives recipients FYI.png" width="400" height="200">
</kbd>

## Freshservice setup

1) Click "Admin" on the bottom left, Click "Form Fields". Add a boolean at the bottom of the list and label it "Engage with xMatters and click "Save". This box is to trigger and engage additional on-call resources to help work on the incident (i.e invite to Slack channel/MS Teams, spin up a Zoom bridge and invite people, etc.)

<kbd>
  <img src="media/Admin form fields.png" width="400" height="200">
</kbd>

<kbd>
  <img src="media/Ticket Engage with xMatters.png" width="400" height="200">
</kbd>

2) Click "Admin" on the bottom left, Click "Workflow Automator" under the Helpdesk Productivity section.

<kbd>
  <img src="media/Workflow Automator.png" width="400" height="200">
</kbd>

3) Click "New Automator" on the top right of the screen. Then click Ticket and give it a name (i.e. "xMatters Trigger") and a Description (i.e. "Will trigger a xMatters webhook when a high priority incident is raised")
4) Drag and drop the appropriate steps onto the automator to have it look like this:

<kbd>
  <img src="media/Workflow Automator xMatters Trigger.png" width="400" height="200">
</kbd>

**NOTE** You can change the Priority to what you want. In my example, I only have Urgent and High Priority Tickets.

5) When you get to "Trigger Webhook" Section, copy and paste the xMatters URL you got from step 5 in the xMatters set up into the "Callback URL" section.

<kbd>
  <img src="media/Webhook section Freshservice.png" width="400" height="200">
</kbd>

6) Scroll down to the Content Section and check all the boxes available and click "Done". Click Activate on the top right of the screen, then done.

7) Repeat steps 3-6 using the screenshot example below, and label it "Engage with xMatters". This time, paste the xMatters URL you got in step 7 (xMatters Setup) in the "Callback URL" section when setting up the Webhook.

<kbd>
  <img src="media/Engage checkbox automator.png" width="400" height="200">
</kbd>

**NOTE** This is making it so when that checkbox is checked in a ticket, it will trigger a xMatters event and any additional steps you have made in the flow. For now, it is a simple notification with the ticket information.

8) Repeat steps 3-6 using the screenshot example below, and label it "Inform with xMatters". This time, paste the xMatters URL you got in step 8 (xMatters Setup) in the "Callback URL" section when setting up the Webhook.

<kbd>
  <img src="media/Inform checkbox automator.png" width="400" height="200">
</kbd>

**NOTE** This will make it so you can add a public note in a Freshservice incident and will trigger a formatted notification with that information out.

## General Notes
1) Under the forms section in the Workflow, please make sure your API user has "Sender Permissions" to execute what it needs to. To find "Sender Permissions", click on "Web Services" dropdown, click on "Sender Permssions", then add your API user into the list.

<kbd>
  <img src="media/Sender Permission location.png" width="400" height="200">
</kbd>

<kbd>
  <img src="media/List of sender permission.png" width="200" height="200">
</kbd>

2) 

# Troubleshooting
 If it doesn't work, you can reference the "Activity Stream" of the integration. Please reference both screen shots to find the Activity Stream and what it should look like. You can look through the logs to see what the error is.


The repo should have the following files:
* **README.md** - This is the main page of the repo and will contain all the instructions and links. See below for document structure
* **LICENSE** - This is the license to cover copying and such. Just copy the contents of [this file](https://github.com/xmatters/xm-labs-template/blob/master/LICENSE)
* **/media/*** - Directory for holding screenshots or videos
* As needed:
   * **Workflow.zip** - Helpful for packaging one or more steps for easy import. Also necessary for any forms and templates
   * **somescript.js** - If it makes more sense to just include a javascript file that gets copy/pasted then name it appropriately

The **README.md** file should have the following structure and contents
* **Name**
   * Quick, two line description of the entry
   * Link to disclaimer image: https://raw.githubusercontent.com/xmatters/xMatters-Labs/master/media/disclaimer.png. Note that when clicked, the browser is sent here: https://support.xmatters.com/hc/en-us/community/topics
* **Pre-Requisites**
   * A list of pre-reqs to use the entry. Versions where applicable are helpful. 
* **Files**
   * A list of necessary files. If there is a workflow.zip file add an entry here with a link. 
* **How it works**
   * An in-depth description of how it works. Especially any tricky or confusing bits
* **Installation**
   * Installation details. Use whatever order makes sense
   * **xMatters**
   * **"Other"**
* **Testing**
   * How do I know it's working? What should I expect?
* **Troubleshooting**
   * Point to docs in the help walk through the pieces of the "pipeline" and where to inspect for errors or broken bits

### Clone the template
*Note*: These instructions use git in the terminal. The GitHub desktop client is rather limited and likely won't save you any headaches. 

Open a command line and do the following. Where `MY_NEW_REPO_NAME_HERE` is the name of your GitHub repo and `MY_NEW_REPO_URL` is the url generated when you create the new repo. 

```bash
# Clone the template repo to the local file system. 
git clone https://github.com/xmatters/xm-labs-template.git
# Change the directory name to avoid confusion, then cd into it
mv xm-labs-template MY_NEW_REPO_NAME_HERE
cd MY_NEW_REPO_NAME_HERE
# Remove the template git history
rm -Rf .git/
# Initialize the new git repo
git init
# Point this repo to the one on GitHub
git remote add origin https://github.com/MY_NEW_REPO_URL.git
# Add all files in the current directory and commit to staging
git add .
git commit -m "initial commit"
# Push to cloud!
git push origin master
```

## 3. Make updates
Then, make the updates to the `README.md` file and add any other files necessary. `README.md` files are written in GitHub-flavored markdown, see [here](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) for a quick reference. 


## 4. Push to GitHub
Periodically, you will want to do a `git commit` to stash the changes locally. Then, when you are ready or need to step away, do a `git push origin master` to push the local changes to github.com. 

## 5. Request to add to xM Labs
Once you are all finished, let Travis know and he will then fork it to the xMatters account and update the necessary links in the xM Labs main page. From there if you update your repo, those changes can be merged into the xMatters account repo and everything will be kept up to date!

# Template below:
---

# Product Name Goes Here
A note about what the product is and what this integration/scriptlet is all about. Check out the sweet video [here](media/mysweetvideo.mov). Be sure to indicate what type of integration or enhancement you're building! (One-way or closed-loop integration? Script library? Feature update? Enhancement to an existing integration?)

<kbd>
  <a href="https://support.xmatters.com/hc/en-us/community/topics"><img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png"></a>
</kbd>

# Pre-Requisites
* Version 453 of App XYZ
* Account in Application ABC
* xMatters account - If you don't have one, [get one](https://www.xmatters.com)!

# Files
* [ExampleCommPlan.zip](ExampleCommPlan.zip) - This is an example comm plan to help get started. (If it doesn't make sense to have a full communication plan, then you can just use a couple javascript files like the one below.)
* [EmailMessageTemplate.html](EmailMessageTemplate.html) - This is an example HTML template for emails and push messages. 
* [FileA.js](FileA.js) - An example javascript file to be pasted into a Shared Library in the Integration builder. Note the comments

# How it works
Add some info here detailing the overall architecture and how the integration works. The more information you can add, the more helpful this sections becomes. For example: An action happens in Application XYZ which triggers the thingamajig to fire a REST API call to the xMatters inbound integration on the imported communication plan. The integration script then parses out the payload and builds an event and passes that to xMatters. 

# Installation
Details of the installation go here. 

## xMatters set up
1. Steps to create a new Shared Library or (in|out)bound integration or point them to the xMatters online help to cover specific steps; i.e., import a communication plan (link: http://help.xmatters.com/OnDemand/xmodwelcome/communicationplanbuilder/exportcommplan.htm)
2. Add this code to some place on what page:
   ```
   var items = [];
   items.push( { "stuff": "value"} );
   console.log( 'Do stuff' );
   ```


## Application ABC set up
Any specific steps for setting up the target application? The more precise you can be, the better!

Images are encouraged. Adding them is as easy as:
```
<kbd>
  <img src="media/cat-tax.png" width="200" height="400">
</kbd>
```

<kbd>
  <img src="media/cat-tax.png" width="200" height="400">
</kbd>


# Testing
Be specific. What should happen to make sure this code works? What would a user expect to see? 

# Troubleshooting
Optional section for how to troubleshoot. Especially anything in the source application that an xMatters developer might not know about, or specific areas in xMatters to look for details - like the Activity Stream?
