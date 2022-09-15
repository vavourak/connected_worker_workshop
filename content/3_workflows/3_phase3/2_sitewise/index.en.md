---
title : "SiteWise Dashboards (IAM)"
weight : 20
---

### Effort: 1 person

AWS IoT SiteWise not only stores our asset telemetry, but also provides an easy-to-use no-code dashboard portal to view our telemetry in near real-time.  In this workflow, we are using AWS IAM users to build a user pool and manage access permissions.

There are 2 main steps we need to accomplish here:
- Create IAM users.
- Instantiate the portal.


### Step 1:

Go to **IAM** (Identity and Access Management), click on **Users**, then finally **Add User**.

![Step 1](/static/sitewisemonitor/25.png)

### Step 2:

Give your user a name.  Under **AWS access type**, select **Password**.
Next to **Console Password**, select **Customer password** and enter a new password for the user.  Deselect **Require password reset**, we are the user, and click **Next**.

![Step 2](/static/sitewisemonitor/26.png)

### Step 3:

In this permissions window, select the **attach existing policies directly**.  We will attach just enough access to the IAM user they can only use the SiteWise portal.

![Step 3](/static/sitewisemonitor/27.png)

### Step 4:

Search for policies that have the word **sitewise**, then select the two policies **AWSIoTSiteWiseMonitorPortalAccess** and **AWSIoTSiteWiseReadOnlyAccess**.  Click Next until we have created the user.

![Step 4](/static/sitewisemonitor/28.png)

### Step 5:

Go to IoT SiteWise, then Portal on the left sidebar.
Click Create Portal.

![Step 1](/static/sitewisemonitor/1.png)

### Step 6:

Give your portal a name.

![Step 2](/static/sitewisemonitor/2.png)

### Step 7:

Under **User authentication**, select IAM.

![Step 7](/static/sitewisemonitor/33.png)

### Step 8:

Next we need to add a support email.  Pick a team member’s email, we will not be needing it.

![Step 8](/static/sitewisemonitor/5.png)

### Step 9:

We will let IoT SiteWise generate the needed IAM service role to manage the dashboard portal.
Click Next.

![Step 6](/static/sitewisemonitor/6.png)

### Step 10:

On the next screen, we will turn off Alarms, as we are using IoT Events for them instead.
Click Create.  We have created the portal, but we need to add admins and users.

![Step 7](/static/sitewisemonitor/7.png)

### Step 11:

Lets add the IAM user we created earlier as an admin.  Select the user from the list.

![Step 8](/static/sitewisemonitor/36.png)

### Step 12:

Next page is for normal users, let’s add the user again.

![Step 9](/static/sitewisemonitor/37.png)

### Step 13:

IoT SiteWise Portal is complete!  
Copy the URL and open it in a new browser tab.

![Step 10](/static/sitewisemonitor/10.png)

### Step 14:

At the AWS login screen, we need to select **IAM user** as the sign-in method.  

![Step 11](/static/sitewisemonitor/39.png)

We will need the 12-digit AWS account number, which can be found by going back to the AWS console tab, click on the account number in the top-right corner, and clicking the copy button next to the account number.

![Step 11](/static/sitewisemonitor/40.png)

### Step 15:

Once completed, we can now log into IoT SiteWise Portal.

![Step 12](/static/sitewisemonitor/41.png)

### Step 16:

Congrats!  WE are now logged in. Look through the Getting Started guide and let’s explore.
One of the first things we should do is check out the Assets screen.  It will automatically have graphs of the telemetry coming in.  Let’s confirm that we are receiving data.

![view assets](/static/sitewisemonitor/13.png)

### Step 17:

In order to build a dashboard, it has to be attached to a project.  With a single portal, we can integrate multiple IoT projects without the need for separate portal instances.  We only have 1 project for one, so lets create it.  Click on **Projects** on the left sidebar, then **Create Project**.

![create project](/static/sitewisemonitor/14.png)

### Step 18:

In the pop-up window, let's give it a name and description.  E.g. **Connected Worker**.
Click **Create Project**.

![name project](/static/sitewisemonitor/15.png)

### Step 19:

Now that we have a project, we need to both assign assets to the project and SSO users.
First, let's assign the users.  While still in the **Connected Worker** project details, scroll down and click **Add Owners**.

![add owners](/static/sitewisemonitor/17.png)

### Step 20:

In the pop-up window, select the users on the left where it says **Portal Users** using the checkbox, then use the double-right arrows to move the users to the right where it says **Project Owners**.  
Click **Save** once done.

![select owners](/static/sitewisemonitor/18.png)

### Step 21:

Next, we need to assign our Connected Worker asset to this project.  Click on the **Assets** button from the left sidebar.
Then click on the asset (might be selected by default) and click **Add Asset to Project**.

![add to project](/static/sitewisemonitor/19.png)

### Step 22:

In the following pop-up, select the asset, then **Select Excisting Project**, then select our project from the drop-down list.
Once done, click **Add Asset to Project**.

![add to project2](/static/sitewisemonitor/20.png)

### Step 23:

Now we can create a dashboard.  While still in the **Connected Worker** project details, click on **Create Dashboard**.

![create dash](/static/sitewisemonitor/16.png)

### Step 24:

Time to get creative!  You can explore the asset hierarchy on the right side, select the different properties, and drag-n-drop them into the middle to visualize them.
At the top you can assign the dashboard a name and in the top-right you can save your progress.

![create dash](/static/sitewisemonitor/21.png)

Example of a basic dashboard.

![create dash](/static/sitewisemonitor/22.png)
