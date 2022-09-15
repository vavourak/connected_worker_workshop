---
title : "Message Routing - Watches"
weight : 20
---

### Effort: 1-2 people
- Person 1: Watch 1 rule
- Person 2: Watch 2 rule

We need to forward the sensor telemetry from IoT Core’s MQTT broker to the different AWS services, like IoT Events and IoT SiteWise.  Therefore we will use the Message Routing rules available to us in IoT Core.

### Step 1:
Let’s go to the **Rules** section and create a rule.
![Step 1](/static/rules/1.png)

### Step 2:
Now let’s create a rule for the watches.  We need one rule per watch, so two people can tackle this workflow at the same time.
Click **Create Rule**.
![Step 20](/static/rules/20.png)

### Step 3:
Give the rule the name **watch_?_forwarding**, adjusting the **?** to the watch number.
Click **Next**.
![Step 21](/static/rules/21.png)

### Step 4:
Enter the following SQL statement:

**SELECT * FROM 'Connected_Worker/Group_01/Worker_Watch_1/up'**

Again, adjust the statement accordingly to the watch number.  
Click **Next**.
![Step 22](/static/rules/22.png)

### Step 5:
Now we need to do something with these messages.
Lets first send the data to **IoT Events**.
Select it from the drop-down list.
Select the **Worker_Watches** input.
![Step 23](/static/rules/23.png)

For IAM role, select the **WatchRuleRole** entry from the drop-down.
![Step 24](/static/rules/24.png)

Next, let's forward the data to IoT SiteWise assets.
Click **Add Rule Action** to allow another action.
![Step 8](/static/rules/8.png)

### Step 6:
Select **Send a message data to asset properties in AWS IoT SiteWise** from the drop-down list.
![Step 9](/static/rules/9.png)

### Step 7:
Lets link the data to property aliases.  There are 4 entries we need to fill out, clicking **Add Entry** to create more entries.  Do **not** click Add Row.

Property Aliases (adjust watch number as needed):
**/watch1/gas_reading**
- Time in Seconds: **$\{time\}**
- Data Type: **INTEGER**
- Value: **$\{gas_reading\}**

**/watch1/gas_alarm_threshold**
- Time in Seconds: **$\{time\}**
- Data Type: **INTEGER**
- Value: **$\{gas_alarm_threshold\}**

**/watch1/own_alarm**
- Time in Seconds: **$\{time\}**
- Data Type: **INTEGER**
- Value: **$\{own_alarm\}**

**/watch1/alarm_state**
- Time in Seconds: **$\{time\}**
- Data Type: **INTEGER**
- Value: **$\{alarm_state\}**
![Step 25](/static/rules/25.png)
![Step 26](/static/rules/26.png)
![Step 27](/static/rules/27.png)

### Step 8:
For IAM role, select the **WatchRuleRole** entry from the drop-down.
Click **Next** then **Create**.
![Step 28](/static/rules/28.png)

Our rule should now be active.  Data from the watches should now be flowing into IoT Events and IoT SiteWise. 