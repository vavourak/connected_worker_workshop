---
title : "Message Routing - Base Station"
weight : 10
---

### Effort: 1 person

We need to forward the sensor telemetry from IoT Core’s MQTT broker to the different AWS services, like IoT Events and IoT SiteWise.  Therefore we will use the Message Routing rules available to us in IoT Core.

### Step 1:
Let’s go to the **Rules** section and create a rule.
![Step 1](/static/rules/1.png)

### Step 2:
Let’s make a rule for the base station data routing.
Let’s name the rule **base_station_forwarding**. 
Click **Next**.
![Step 2](/static/rules/2.png)

### Step 3:
Now we can write an SQL statement to filter which messages fall into this rule.  We will take all messages and data coming from the base station.

Enter the following SQL statement:

**SELECT * FROM 'Connected_Worker/Base_Station_??/manager'**

Replace the ?? with your base station’s number.
![Step 3](/static/rules/3.png)

### Step 4:
Now we need to do something with these messages.
Lets first send the data to **IoT Events**.
Select it from the drop-down list.
Select the **Base_Station** input.
![Step 4](/static/rules/4.png)

For IAM role, select the **BaseStationRuleRole** entry from the drop-down.
![Step 7](/static/rules/7.png)

### Step 5:
Next, let's forward the data to IoT SiteWise assets.
Click **Add Rule Action** to allow another action.
![Step 8](/static/rules/8.png)

### Step 6:
Select **Send a message data to asset properties in AWS IoT SiteWise** from the drop-down list.
![Step 9](/static/rules/9.png)

### Step 7:
Lets link the data to property aliases.  There are 4 entries we need to fill out, clicking **Add Entry** to create more entries.  Do **not** click Add Row.

Property Aliases (adjust base name as needed):
**/base??/gas_reading**
- Time in Seconds: **$\{time\}**
- Data Type: **INTEGER**
- Value: **$\{gas_reading\}**

**/base??/gas_alarm_threshold**
- Time in Seconds: **$\{time\}**
- Data Type: **INTEGER**
- Value: **$\{gas_alarm_threshold\}**

**/base??/own_alarm**
- Time in Seconds: **$\{time\}**
- Data Type: **INTEGER**
- Value: **$\{own_alarm\}**

**/base??/alarm_state**
- Time in Seconds: **$\{time\}**
- Data Type: **INTEGER**
- Value: **$\{alarm_state\}**

![Step 10](/static/rules/10.png)
![Step 11](/static/rules/11.png)
![Step 12](/static/rules/12.png)
![Step 13](/static/rules/13.png)

### Step 8:
For the IAM role, select the same **BaseStationRuleRole** as before.
![Step 14](/static/rules/14.png)

### Step 9:
Now let’s review everything and click **Create**.
![Step 15](/static/rules/15.png)

Our rule should now be active.  Data from the base station should now be flowing into IoT Events and IoT SiteWise. 
![Step 18](/static/rules/18.png)
