---
title : "IoT SiteWise - Build a Data Twin"
weight : 40
---

### Effort: 1-2 people
- Person 1: Steps 1-7, 13- (Base Station & Grouping)
- Person 2: Steps 8-12 (Watches)

We need somewhere to store our sensor telemetry, so that we can tie it to actual assets and visualize it.  IoT SiteWise is purpose-built for for this use-case.

### What is IoT SiteWise?
AWS IoT SiteWise is a managed service that makes it easy to collect, store, organise and monitor data from industrial equipment at scale.
- Collect, manage, and visualize data from all your industrial equipment sources without developing additional software.
- Optimize processes across your facility portfolio with insights from automatic, customizable data visualizations.
- Collect and process industrial data locally, and build hybrid industrial applications that work seamlessly across the edge and cloud.

The general  flow is as follows:
- Create models of the different types of hardware, i.e. the watch and base station.
- Create actual assets from these model templates and assign aliases to the different attributes so we can forward data to them.

### Step 1:
Go to **IoT SiteWise**, then the **Models** section.  We need to create model templates of our watch and base station.
Click **Create Model**.
![Step 1](/static/sitewisemodels/1.png)

### Step 2:
First, we will model our base station.  Enter the model name **Base Station**.
![Step 2](/static/sitewisemodels/2.png)

Add measurements definitions, i.e. what this model should expect as telemetry.  Fill out the form as shown below, all should be of **Integer** data type. We need 4 attributes: 
- Gas Reading
- Alarm Threshold
- Device Alarm State
- General Alarm State.

Click **Create** when done.
![Step 3](/static/sitewisemodels/3.png)

### Step 3:
Now we have a model template for base stations.  It can take a minute or so to create it.
![Step 4](/static/sitewisemodels/4.png)

After a minute or so, we can see it is now active. 
![Step 5](/static/sitewisemodels/5.png)

### Step 4:
Now lets make an actual asset based off this model.  
Click **Assets**, then **Create Asset**.
![Step 6](/static/sitewisemodels/6.png)

### Step 5:
We will create an asset based off the Base Station model and give it the name **Base Station xx**, according to the number on the device.
Once done, click **Create Asset**.
![Step 7](/static/sitewisemodels/7.png)

### Step 6:
Wait a minute until it becomes active. 
Before we can use the asset, we need to define aliases for the 4 measurements, so we can route data to them.
Once active, we can click the **Edit** button.
![Step 8](/static/sitewisemodels/8.png)

### Step 7:
Fill out the aliases as shown below, adjusting to your group number accordingly.
- /base01/gas_reading
- /base01/gas_alarm_threshold
- /base01/own_alarm
- /base01/alarm_state
Click **Save** once done.
![Step 9](/static/sitewisemodels/9.png)

### Step 8:
Now lets repeat the process, but for the watches.
Lets create a new model with the name **Worker Watch**. 
![Step 10](/static/sitewisemodels/10.png)

We will add the following measurement definitions.  Fill out the form as shown below, all should be of **Integer** data type. We need 4 attributes: 
- Gas Reading
- Alarm Threshold
- Device Alarm State
- General Alarm State.

Click **Create Model** when done.
![Step 11](/static/sitewisemodels/11.png)

Wait until Active, then move to the next step.
![Step 12](/static/sitewisemodels/12.png)

### Step 9:
Now we will make multiple watch assets, one for each of our watches.
Click **Create Asset**. 
![Step 13](/static/sitewisemodels/13.png)

### Step 10:
Select **Worker Watch** as the model and give it the name **Worker Watch ?**, replacing the **?** with either 1 or 2.
Click **reate Asset**.
![Step 14](/static/sitewisemodels/14.png)

### Step 11:
Wait a minute until it becomes active. 
Before we can use the asset, we need to define aliases for the 4 measurements, so we can route data to them.
Once active, we can click the **Edit** button.
![Step 15](/static/sitewisemodels/15.png)

### Step 12:
Fill out the aliases as shown below, adjusting to your group number accordingly.
- /watch1/gas_reading
- /watch1/gas_alarm_threshold
- /watch1/own_alarm
- /watch1/alarm_state
Click **Save** once done.
![Step 16](/static/sitewisemodels/16.png)

### Step 13:
We are done creating the assets!  But there is one more step that we need to do in order to easily make dashboards later.  We need to group these assets together into a hierarchy.  Similar to actual assets, the hierarchy also has a model and asset.  
Let's made the model first.  Go to **Models** on the left sidebar, then click **Create Model**.
![hier model](/static/sitewisemodels/17.png)

### Step 14:
Now we need to give it a name, e.g. **Connected Worker**.
![hier name](/static/sitewisemodels/18.png)

### Step 15:
Next we need to assign the hierachy, which other models below to this one.  Scroll down some more and find the **Hierachy Definitions** section.
We will assign both the **Base Station** and **Worker Watch** models to this model.
![hier models](/static/sitewisemodels/19.png)
![hier models2](/static/sitewisemodels/20.png)

### Step 16:
Now we need to make an actual asset out of the model.  Go to **Assets** then **Create Asset**.
![hier asset](/static/sitewisemodels/21.png)

### Step 17:
Create a new asset based off our **Connected Worker** model and give it a name, e.g. **Connected Worker** again.
![hier asset2](/static/sitewisemodels/22.png)

### Step 18:
Click the **Edit** button so we can assign the other assets to this one.
![hier edit](/static/sitewisemodels/23.png)

### Step 19:
Scroll down to **Assets associated to this asset**.  Click **Add associated asset** and add our 3 assets, just like in the picture.
Click **Save** when done.
![hier edit](/static/sitewisemodels/24.png)

We are done!  We should now see that the base station and watch assets are nested inside of our Connected Worker asset.  If it doesn't show up, pressing refresh on the browser will update the display correctly.
![hier](/static/sitewisemodels/25.png)
