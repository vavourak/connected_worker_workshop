---
title : "SiteWise Dashboards (SSO)"
weight : 10
---

### Effort: 1 person

AWS IoT SiteWise not only stores our asset telemetry, but also provides an easy-to-use no-code dashboard portal to view our telemetry in near real-time.  It uses AWS Single-Sign-On (AWS SSO) to build a user pool and manage access permissions, which is useful, as we can use reuse this user-pool for other AWS services too, like Amazon Managed Service for Grafana.

There are 2 main steps we need to accomplish here:
- Instantiate the portal.
- Create and assign SSO users.


### Step 1:
Go to IoT SiteWise, then Portal on the left sidebar.
Click Create Portal.

![Step 1](/static/sitewisemonitor/1.png)

### Step 2:
Give your portal a name.
![Step 2](/static/sitewisemonitor/2.png)

### Step 3:
Need usernames so we can log in.  Using AWS SSO.
Click Create User.
![Step 3](/static/sitewisemonitor/3.png)

### Step 4:
We will need to make accounts for each member of your group, but first we will make only a single account now.  
Enter email and name of one person here and click Create User.
![Step 4](/static/sitewisemonitor/4.png)

### Step 5:
Next we need to add a support email.  Pick a team member’s email, we will not be needing it.
![Step 5](/static/sitewisemonitor/5.png)

### Step 6:
We will let IoT SiteWise generate the needed IAM service role to manage the dashboard portal.
Click Next.
![Step 6](/static/sitewisemonitor/6.png)

### Step 7:
On the next screen, we will turn off Alarms, as we are using IoT Events for them instead.
Click Create.
![Step 7](/static/sitewisemonitor/7.png)

### Step 8:
Here we can now add all the team members and invite them to the portal as admins.
![Step 8](/static/sitewisemonitor/8.png)

### Step 9:
Next page is for users, let’s add everyone again.
![Step 9](/static/sitewisemonitor/9.png)

### Step 10:
IoT SiteWise Portal is complete!  
Copy the URL and open it in a new browser tab.
![Step 10](/static/sitewisemonitor/10.png)

### Step 11:
Before we log in, we need to confirm our AWS SSO account.  By now, you should have received an email from AWS Single Sign-On with an Accept Invitation button.  Click it.
This will take you to the AWS SSO page to create a password.  Do so.
![Step 11](/static/sitewisemonitor/11.png)

### Step 12:
Once completed, we can now log into IoT SiteWise Portal.
![Step 12](/static/sitewisemonitor/12.png)

### Step 13:
Congrats!  You are now logged in. Look through the Getting Started guide and let’s explore.
One of the first things we should do is check out the Assets screen.  It will automatically have graphs of the telemetry coming in.  Let’s confirm that we are receiving data.
![view assets](/static/sitewisemonitor/13.png)

### Step 14:
In order to build a dashboard, it has to be attached to a project.  With a single portal, we can integrate multiple IoT projects without the need for separate portal instances.  We only have 1 project for one, so lets create it.  Click on **Projects** on the left sidebar, then **Create Project**.
![create project](/static/sitewisemonitor/14.png)

### Step 15:
In the pop-up window, let's give it a name and description.  E.g. **Connected Worker**.
Click **Create Project**.
![name project](/static/sitewisemonitor/15.png)

### Step 16:
Now that we have a project, we need to both assign assets to the project and SSO users.
First, let's assign the users.  While still in the **Connected Worker** project details, scroll down and click **Add Owners**.
![add owners](/static/sitewisemonitor/17.png)

### Step 17:
In the pop-up window, select the users on the left where it says **Portal Users** using the checkbox, then use the double-right arrows to move the users to the right where it says **Project Owners**.  
Click **Save** once done.
![select owners](/static/sitewisemonitor/18.png)

### Step 18:
Next, we need to assign our Connected Worker asset to this project.  Click on the **Assets** button from the left sidebar.
Then click on the asset (might be selected by default) and click **Add Asset to Project**.
![add to project](/static/sitewisemonitor/19.png)

### Step 19:
In the following pop-up, select the asset, then **Select Excisting Project**, then select our project from the drop-down list.
Once done, click **Add Asset to Project**.
![add to project2](/static/sitewisemonitor/20.png)

### Step 20:
Now we can create a dashboard.  While still in the **Connected Worker** project details, click on **Create Dashboard**.
![create dash](/static/sitewisemonitor/16.png)

### Step 21:
Time to get creative!  You can explore the asset hierarchy on the right side, select the different properties, and drag-n-drop them into the middle to visualize them.
At the top you can assign the dashboard a name and in the top-right you can save your progress.
![create dash](/static/sitewisemonitor/21.png)

Example of a basic dashboard.
![create dash](/static/sitewisemonitor/22.png)
