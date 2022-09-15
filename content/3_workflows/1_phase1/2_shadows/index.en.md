---
title : "Device Shadows - Define Alarm Thresholds"
weight : 20
---

### Effort: 1-3 people
- Person 1: Steps 1-8 (Base station shadow)
- Person 2: Steps 9-13 (Watch 1 shadow)
- Person 3: Steps 9-13 (Watch 2 shadow)


### What are device shadows?
The AWS IoT Device Shadow service adds shadows to AWS IoT thing objects. Shadows can make a device’s state available to apps and other services whether the device is connected to AWS IoT or not. 

Why are we using one today?  Well, every gas sensor is different, even of the same type.  It will have a different base-line, response profile and even different warm-up time.  As such, an alarm threshold for one device will not work for another.  We will use Device Shadows to assign unique thresholds to each device.  Each device is programmed to query it's shadow state to retrieve it's updated threshold threshold.  With this, we can adjust the threshold if needed right from the AWS Console by editing the device's shadow.

### Note:
We will likely have to return and adjust thresholds at a later point, so keep this workflow handy.  Also, the gas sensor in the base station works differently than the ones in the watches.  The base station's sensor decreases in ohms reading as gas concentrations increase, while the watches increases.  This we need to set the base station threshold below its baseline readings and the watches above it.


### Step 1:
First, let's create a shadow for our base station.
Go to **Greengrass** and then **Core Devices**.
Click on our base station entry.
![Step 1](/static/shadows/1.png)

### Step 2:
Here, there will be a link to the IoT Thing associated with this core device.  Click it.
![Step 2](/static/shadows/2.png)

### Step 3:
Click the **Device Shadows** tab, then click **Create Shadow**.
![Step 3](/static/shadows/3.png)

### Step 4:
We will create a **Named Shadow**, with the name **State**.
![Step 4](/static/shadows/4.png)

### Step 5:
Now that it has been created, lets click on it.
![Step 5](/static/shadows/5.png)

### Step 6:
We need to edit it to add our threshold.  Click **Edit**.
![Step 6](/static/shadows/6.png)

### Step 7:
We will set the desired threshold by adding a row.  Here we can check the readings of the sensors to see what baseline is reasonable, but some experimentation is probably needed.
**"gas_alarm_threshold":8000**
Don't forget to add the comma on the line above.
![Step 7](/static/shadows/7.png)

### Step 8 (Optional):
We can now check on the MQTT feed from the device and see if the new threshold has been received.  The device relays its current threshold back. 
![Step 8](/static/shadows/8.png)

### Step 9:
When the watches were provisioned as IoT Things, the shadow was created automatically.  However, it does not have the gas_alarm_threshold entry, so we will need to add it.

Go to **Things**, then click on one of our watches.
![Step 9](/static/shadows/9.png)

### Step 10:
Click on the **Device Shadows** tab, then click on the **State** shadow.
![Step 10](/static/shadows/10.png)

### Step 11:
Click **Edit**.
![Step 11](/static/shadows/11.png)

### Step 12:
We will set the desired threshold by adding a row.  Here we can check the readings of the sensors to see what baseline is reasonable, but some experimentation is probably needed.
**"alarm_threshold":8000**
Don't forget to add the comma on the line above.
![Step 12](/static/shadows/12.png)

### Step 13 (Optional):
We can now check on the MQTT feed from the device and see if the new threshold has been received.  The device relays its current threshold back.
![Step 13](/static/shadows/13.png)
![Step 14](/static/shadows/14.png)
