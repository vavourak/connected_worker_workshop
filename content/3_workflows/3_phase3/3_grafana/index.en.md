---
title : "Grafana Dashboards"
weight : 20
---

### Effort: 1 person

### What is Amazon Managed Grafana?

Amazon Managed Grafana is a fully managed service for open source Grafana developed in collaboration with Grafana Labs. Grafana is a popular open source analytics platform that enables you to query, visualize, alert on and understand your metrics no matter where they are stored.

### Step 1:

Let's go to Amazon Managed Grafana and click **Create Workspace**.

![Create Workspace](/static/grafana/01.png)

### Step 2:

Give it a name, like **Connected_Worker_Workspace** and click **Next**.

![Workspace name](/static/grafana/02.png)

### Step 3:

Enable AWS SSO.  If we have completed the SiteWise dashboard workflow, then AWS SSO will have all the users that we need.  They will share the same SSO user pool.  Click **Next**.

![enable sso](/static/grafana/03.png)

### Step 4:

From data sources, select **AWS IoT SiteWise**.  Grafana can ingest data from many AWS services, but in this workshop we will only use SiteWise data.  Click **Next**.

![datasource sitewise](/static/grafana/04.png)

### Step 5:

Click **Create Workspace**.

![Create Workspace](/static/grafana/05.png)

### Step 6:

It will take a minute or two to provision the resources and create the workspace.

![pending create](/static/grafana/06.png)

Once done, we need to assign SSO users to this workspace.  Once the SiteWise workflow is done, then this will be fully populated.  Click the **Assign new user or group**.

![assign sso users](/static/grafana/07.png)

Select the users and then **Assign users and groups**.

![add user](/static/grafana/08.png)

### Step 7:

The users are added, but not given any permissions yet.  Let's select all users and then using the **Action** button, assign them as admins by selecting **Make Admin**.

![make admin](/static/grafana/09.png)

### Step 8:

Now let's visit our Managed Grafana website.  Click the URL.

![open url](/static/grafana/10.png)

### Step 9:

This is the AWS SSO login page.  Once you have verified your email account and assigned yourself a password, we should be able to log in.
![sign in sso](/static/grafana/11.png)
![sign in sso](/static/grafana/12.png)

### Step 10:

Once we are in Grafana, we need to enable the SiteWise data source.
Hover your mouse over the **AWS** logo on the left sidebar and select **data sources**.
![enable datasource](/static/grafana/13.png)

Select **IoT SiteWise** as the service and **US East (N. Virginia)** as the region.

![add our sitewise](/static/grafana/14.png)

### Step 11:

Now we can start creating dashboards!  Hover the mouse over the **+** symbol on the left sidebar and select **Dashboards**.

![create dashboard](/static/grafana/15.png)

### Step 12:

There will be a default blank section where we want to click **Add a new panel**.

![add panel](/static/grafana/16.png)

### Step 13:

Now let's data to this panel.  We will start with showing a chart of the base station's gas readings over time.  
Select **AWS IoT SiteWise** from the **Data source** row, then for **Query type** we select **Get property value history**.  Next to **Asset**, we select our SiteWise asset **Connected Worker** and then **Explore**.  Our base asset does not have the telemetry itself (though it could aggregate it if needed), so we need to pull data from one of the sub assets.

![panel data](/static/grafana/17.png)

### Step 13:

In the Asset Browser pop-up, select **Base Station**.

![find asset](/static/grafana/18.png)

### Step 14:

We want to show the **Gas Reading** property.  Confirm that everything looks correct as in the picture.  When done, click **Apply**.

![finalize panel](/static/grafana/19.png)

We are done following the guide, now time to be creative!  Explore Grafana and see how we can visualize the IoT telemetry in useful and creative ways.

Here is a basic example.  Don't forget we can enable auto-refresh so that the dashboard is near-real-time.  We can add more panels, change visual types, aggregate data and more.

![basic example](/static/grafana/20.png)


