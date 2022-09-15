---
title : "IoT Greengrass - Deploy Base Station"
weight : 10
---

### Effort: 1-3 people
- Person 1: Steps 1-11 (BME688 component & deployment)
- Person 2: Steps 1-5 (PiTFT component)
- Person 3: Steps 1-5 (Manager component)

As the base station is running a full operating system, we can use IoT Greengrass to manage it.  We can manage what scripts, called **components**, are deployed to the device from within the AWS Console, along with many other abilities.
In this workflow, we will create 3 components, which are recipes of how to deploy specific code files to the device.  Then we will deploy the components to the base station. Finally, we will verify that the deployment worked.

The 3 scripts we will be using are the following:
- **BME688**:  Interfaces with the Bosch BME688 gas sensor.
- **PiTFT**: Controls the Adafruit PiTFT 1.3" display.
- **Manager**: Manages local communications of components and relays messages back to AWS IoT.

Components deployed to edge devices have 2 mechanisms of communication:  Locally between scripts and over the network to AWS IoT Core.  One design pattern is to have the components that manage the periferal tasks speak locally at high frequency, and have a manager component that manages aggregating the information and communicating it back to AWS as needed.

![Arch](/static/greengrass/a.png)


### Teamwork:
Step 3 contains the first component recipe for the BME688, while step 5 contains the other two.  If more than 1 person is doing this workflow, they will follow the steps 1-4, but grab the appropriate recipe from step 5.

### Step 1:
Our code 3 files have already been uploaded to an S3 bucket on our account.  So let's go check the name of the bucket in which they reside, as we will need to know this in a bit.  Look for a bucket named **remars-base??-greengrass** and take note of it.
![Step 1](/static/greengrass/0.png)

### Step 2:
Now let's go to IoT Greengrass.  On the left sidebar, go to the **Components** section.  We will need to create 3 components, one for each code file.  So let's click **Create Component**.
![Step 2](/static/greengrass/1.png)

### Step 3:
Now let's enter the component **recipe**, which tells the component how it works, it's permissions and where to find the files.  Select the **Enter recipe as YAML** option.  Delete the recipe that is there, as it is stock boilerplate code.

Now we will copy/paste the following recipe for the BME688 script, but make sure to edit the bucket name at the bottom to reflect the S3 bucket in your account:

```yaml
---
RecipeFormatVersion: '2020-01-25'
ComponentName: Base_Station_BME688
ComponentVersion: '1.0.0'
ComponentConfiguration:
  DefaultConfiguration:
    accessControl:
      aws.greengrass.ipc.pubsub:
        "Base_Station_BME688:pubsub:1":
          operations:
            - "*"
          resources:
            - "*"
      aws.greengrass.ipc.mqttproxy:
        "Base_Station_BME688:pubsub:1":
          operations:
            - "*"
          resources:
            - "*"
Manifests:
- Lifecycle:
    Run: python3 {artifacts:path}/connected_worker_base_bme688.py
  Artifacts:
  - Uri: s3://remars-base??-greengrass/connected_worker_base_bme688.py
```

Once done, click the **Create Component** button at the bottom.
![Step 2](/static/greengrass/2.png)

### Step 4:
We should now see that version 1.0.0 is a deployable component.
![Step 3](/static/greengrass/3.png)

### Step 5:
Repeat steps 2-4 for each of the remaining 2 scripts, if you are working alone.  Recipes are below:

Base Station **PiTFT** recipe:
```yaml
---
RecipeFormatVersion: '2020-01-25'
ComponentName: Base_Station_PiTFT
ComponentVersion: '1.0.0'
ComponentConfiguration:
  DefaultConfiguration:
    accessControl:
      aws.greengrass.ipc.pubsub:
        "Base_Station_PiTFT:pubsub:1":
          operations:
            - "*"
          resources:
            - "*"
      aws.greengrass.ipc.mqttproxy:
        "Base_Station_PiTFT:pubsub:1":
          operations:
            - "*"
          resources:
            - "*"
Manifests:
- Lifecycle:
    Run: python3 {artifacts:path}/connected_worker_base_pitft13.py
  Artifacts:
  - Uri: s3://remars-base01-greengrass/connected_worker_base_pitft13.py
```

Base Station **Manager** recipe:
```yaml
---
RecipeFormatVersion: '2020-01-25'
ComponentName: Base_Station_Manager
ComponentVersion: '1.0.0'
ComponentConfiguration:
  DefaultConfiguration:
    accessControl:
      aws.greengrass.ipc.pubsub:
        "Base_Station_Manager:pubsub:1":
          operations:
            - "*"
          resources:
            - "*"
      aws.greengrass.ipc.mqttproxy:
        "Base_Station_Manager:pubsub:1":
          operations:
            - "*"
          resources:
            - "*"
Manifests:
- Lifecycle:
    Run: python3 {artifacts:path}/connected_worker_base_manager.py
  Artifacts:
  - Uri: s3://remars-base01-greengrass/connected_worker_base_manager.py
```

### Step 6:
Now that we have all 3 components created, we can deploy them to the base station.
Let's go to **Deployments** on the left sidebar.  here we should see a deployment for our base station already, which is automatically generated when the device was provisioned beforehand.  Click on it.
![Step 4](/static/greengrass/4.png)

### Step 7:
This deployment simply has the AWS CLI component deployed to the device.  We want to deploy the 3 components we just created, so click **Revise**.
![Step 5](/static/greengrass/5.png)

### Step 8:
The first page allows you to rename the deployment, but we can keep it as is.  Click **Next**.
On the next page, under the **My Components** section, we want to select the 3 components we created.  
**Note:**  If they do not appear, you might need to toggle off the **Show only selected components** option.  
Once done, click **Next** until the end and then **Create** the deployment.
![Step 6](/static/greengrass/6.png)

### Step 9:
Confirm that the deployment has initiated successfully. If you have a Raspberry Pi 4 base station, it should take less than 1 minute to update.  If you have a Raspberry Pi Zero station, it can take up to 5 minutes (sorry, it is hard to source Pi4's these days).
![Step 7](/static/greengrass/7.png)

### Step 10 (Optional):
Check the status of the base station by click on **Core Devices** on the left sidebar.  It should say **Healthy** next to it.
![Step 8](/static/greengrass/8.png)

### Step 11 (Optional):
We can also verify that the deployment was successful by checking to see that messages are flowing in on the MQTT topic of the base station.
Go to the **MQTT Test Client** on the left sidebar.  We want to subscribe to the **Connected_Worker/Base_Station_??/manager** topic, adjusting it to your group number.  You should see a message arriving every second.
![Step 9](/static/greengrass/9.png)
