---
title : "IoT Events - Deploy Alarm Model"
weight : 30
---

### Effort: 1-2 people
- Person 1: Steps 1-4
- Person 2: Steps 5-9 (cannot complete step 8 until person 1 has finished)

### What is IoT Events?
AWS IoT Events is a fully managed service that makes it easy to detect and respond to events from IoT sensors and applications. Events are patterns of data identifying more complicated circumstances than expected, such as changes in equipment when a belt is stuck or motion detectors using movement signals to activate lights and security cameras. 


### Step 1:
Go to **IoT Events**.
First, we need to create data inputs, so we can forward the device messages to them.  Once we create the inputs, they can be used in the event detector model.
Click **Inputs** and then **Create Input**.
![Step 1](/static/iotevents/1.png)

### Step 2:
Give it the name **Worker_Watches**.  This will be our entry for data coming in from the watches.  We need to specify what type of data to expect, which we do in the form of a JSON template of an example MQTT message.

Save the JSON code below to a file. 

```json
{
  "device_id": "Worker_Watch_1",
  "time": 1652455665,
  "gas_reading": 0,
  "alarm_state": 0,
  "own_alarm": 0,
  "alarm_threshold": 0
}
```

Upload this file on this screen, then click **Create**.
![Step 2](/static/iotevents/2.png)

### Step 3:
Done.  Now we need to make the data inputs for the base station, so lets hit Create Input again. 
![Step 3](/static/iotevents/3.png)

### Step 4:
Input name is **Base_Station**.  As above, we need to save the following JSON code as a file.

```json
{
  "device_id": "Base_Station_01",
  "source": "Manager",
  "time": 1652969663,
  "gas_reading": 0,
  "alarm_state": 0,
  "own_alarm": 0,
  "alarm_threshold": 0
}
```

Upload this file and hit **Create**. 
![Step 4](/static/iotevents/4.png)

Now we have both inputs ready to receive data and available to be used in a detector model. 
![Step 5](/static/iotevents/5.png)

### Step 5:
Let's go to the **Detector Model** section and click **Create Detector Model**.
![Step 6](/static/iotevents/6.png)

### Step 6:
We will import a detector model so we do not have to build one from scratch.
![Step 7](/static/iotevents/7.png)

On this next screen, simply click **Import**.
![Step 8](/static/iotevents/8.png)

### Step 7:

Save the code block below to a file.  It contains the entire detector model and the logic we need.  As it is quite a bit of code, use the **Copy** button in the top-right corner of the code block.

```json
{
    "detectorModelDefinition": {
        "states": [
            {
                "stateName": "Offline",
                "onInput": {
                    "events": [],
                    "transitionEvents": [
                        {
                            "eventName": "TurnOn",
                            "condition": "$input.Base_Station.own_alarm == 0 && $input.Worker_Watches.own_alarm == 0\n",
                            "actions": [],
                            "nextState": "Online"
                        },
                        {
                            "eventName": "TurnOnBaseAlarm",
                            "condition": "$input.Base_Station.own_alarm == 1",
                            "actions": [],
                            "nextState": "Alarm_Base"
                        },
                        {
                            "eventName": "TurnOnWorkerAlarm",
                            "condition": "$input.Base_Station.own_alarm == 0 && $input.Worker_Watches.own_alarm == 1",
                            "actions": [],
                            "nextState": "Alarm_Worker"
                        }
                    ]
                },
                "onEnter": {
                    "events": [
                        {
                            "eventName": "CreateIdleTimer",
                            "condition": "true",
                            "actions": [
                                {
                                    "setTimer": {
                                        "timerName": "IdleTimer",
                                        "seconds": 60,
                                        "durationExpression": null
                                    }
                                }
                            ]
                        },
                        {
                            "eventName": "CreateAlarmTimeoutTimer",
                            "condition": "true",
                            "actions": [
                                {
                                    "setVariable": {
                                        "variableName": "AlarmCounter",
                                        "value": "0"
                                    }
                                }
                            ]
                        }
                    ]
                },
                "onExit": {
                    "events": []
                }
            },
            {
                "stateName": "Online",
                "onInput": {
                    "events": [
                        {
                            "eventName": "ResetIdleTimer",
                            "condition": "true",
                            "actions": [
                                {
                                    "resetTimer": {
                                        "timerName": "IdleTimer"
                                    }
                                }
                            ]
                        }
                    ],
                    "transitionEvents": [
                        {
                            "eventName": "EnterBaseAlarm",
                            "condition": "$input.Base_Station.own_alarm == 1",
                            "actions": [],
                            "nextState": "Alarm_Base"
                        },
                        {
                            "eventName": "TurnOff",
                            "condition": "timeout(\"IdleTimer\")",
                            "actions": [
                                {
                                    "clearTimer": {
                                        "timerName": "IdleTimer"
                                    }
                                }
                            ],
                            "nextState": "Offline"
                        },
                        {
                            "eventName": "EnterWorkerAlarm",
                            "condition": "$input.Base_Station.own_alarm == 0 && $input.Worker_Watches.own_alarm == 1\n",
                            "actions": [],
                            "nextState": "Alarm_Worker"
                        }
                    ]
                },
                "onEnter": {
                    "events": [
                        {
                            "eventName": "ShadowUpdate",
                            "condition": "true",
                            "actions": [
                                {
                                    "iotTopicPublish": {
                                        "mqttTopic": "$aws/things/Worker_Watch_1/shadow/name/State/update",
                                        "payload": {
                                            "contentExpression": "('{\"state\": {\"desired\": {\"far_alarm\": 0}}}')",
                                            "type": "JSON"
                                        }
                                    }
                                },
                                {
                                    "iotTopicPublish": {
                                        "mqttTopic": "$aws/things/Worker_Watch_2/shadow/name/State/update",
                                        "payload": {
                                            "contentExpression": "('{\"state\": {\"desired\": {\"far_alarm\": 0}}}')",
                                            "type": "JSON"
                                        }
                                    }
                                }
                            ]
                        }
                    ]
                },
                "onExit": {
                    "events": []
                }
            },
            {
                "stateName": "Alarm_Base",
                "onInput": {
                    "events": [
                        {
                            "eventName": "ResetIdleTimer",
                            "condition": "true",
                            "actions": [
                                {
                                    "resetTimer": {
                                        "timerName": "IdleTimer"
                                    }
                                }
                            ]
                        },
                        {
                            "eventName": "DecrementAlarmCounter",
                            "condition": "$input.Worker_Watches.own_alarm == 0",
                            "actions": [
                                {
                                    "setVariable": {
                                        "variableName": "AlarmCounter",
                                        "value": "$variable.AlarmCounter - 1"
                                    }
                                }
                            ]
                        },
                        {
                            "eventName": "ResetAlarmCounter",
                            "condition": "$input.Worker_Watches.own_alarm == 1",
                            "actions": [
                                {
                                    "setVariable": {
                                        "variableName": "AlarmCounter",
                                        "value": "5"
                                    }
                                }
                            ]
                        }
                    ],
                    "transitionEvents": [
                        {
                            "eventName": "ExitBaseAlarm",
                            "condition": "$input.Base_Station.own_alarm == 0 && $input.Worker_Watches.own_alarm == 0 && $variable.AlarmCounter <= 0",
                            "actions": [],
                            "nextState": "Online"
                        },
                        {
                            "eventName": "TurnOffBaseAlarm",
                            "condition": "timeout(\"IdleTimer\")",
                            "actions": [
                                {
                                    "clearTimer": {
                                        "timerName": "IdleTimer"
                                    }
                                }
                            ],
                            "nextState": "Offline"
                        },
                        {
                            "eventName": "EnterWorkerAlarm",
                            "condition": "$input.Base_Station.own_alarm == 0 && $input.Worker_Watches.own_alarm == 1\n",
                            "actions": [],
                            "nextState": "Alarm_Worker"
                        }
                    ]
                },
                "onEnter": {
                    "events": [
                        {
                            "eventName": "ShadowUpdate",
                            "condition": "true",
                            "actions": [
                                {
                                    "iotTopicPublish": {
                                        "mqttTopic": "$aws/things/Worker_Watch_1/shadow/name/State/update",
                                        "payload": {
                                            "contentExpression": "('{\"state\": {\"desired\": {\"far_alarm\": 1}}}')",
                                            "type": "JSON"
                                        }
                                    }
                                },
                                {
                                    "iotTopicPublish": {
                                        "mqttTopic": "$aws/things/Worker_Watch_2/shadow/name/State/update",
                                        "payload": {
                                            "contentExpression": "('{\"state\": {\"desired\": {\"far_alarm\": 1}}}')",
                                            "type": "JSON"
                                        }
                                    }
                                }
                            ]
                        },
                        {
                            "eventName": "ResetAlarmCounter",
                            "condition": "true",
                            "actions": [
                                {
                                    "setVariable": {
                                        "variableName": "AlarmCounter",
                                        "value": "5"
                                    }
                                }
                            ]
                        }
                    ]
                },
                "onExit": {
                    "events": []
                }
            },
            {
                "stateName": "Alarm_Worker",
                "onInput": {
                    "events": [
                        {
                            "eventName": "ResetIdleTimer",
                            "condition": "true",
                            "actions": [
                                {
                                    "resetTimer": {
                                        "timerName": "IdleTimer"
                                    }
                                }
                            ]
                        },
                        {
                            "eventName": "DecrementAlarmCounter",
                            "condition": "$input.Worker_Watches.own_alarm == 0",
                            "actions": [
                                {
                                    "setVariable": {
                                        "variableName": "AlarmCounter",
                                        "value": "$variable.AlarmCounter - 1"
                                    }
                                }
                            ]
                        },
                        {
                            "eventName": "ResetAlarmCounter",
                            "condition": "$input.Worker_Watches.own_alarm == 1",
                            "actions": [
                                {
                                    "setVariable": {
                                        "variableName": "AlarmCounter",
                                        "value": "5"
                                    }
                                }
                            ]
                        }
                    ],
                    "transitionEvents": [
                        {
                            "eventName": "EnterBaseAlarm",
                            "condition": "$input.Base_Station.own_alarm == 1",
                            "actions": [],
                            "nextState": "Alarm_Base"
                        },
                        {
                            "eventName": "TurnOffWorkerAlarm",
                            "condition": "timeout(\"IdleTimer\")",
                            "actions": [],
                            "nextState": "Offline"
                        },
                        {
                            "eventName": "ExitWorkerAlarm",
                            "condition": "$input.Base_Station.own_alarm == 0 && $input.Worker_Watches.own_alarm == 0 && $variable.AlarmCounter <= 0",
                            "actions": [],
                            "nextState": "Online"
                        }
                    ]
                },
                "onEnter": {
                    "events": [
                        {
                            "eventName": "ShadowUpdate",
                            "condition": "true",
                            "actions": [
                                {
                                    "iotTopicPublish": {
                                        "mqttTopic": "$aws/things/Worker_Watch_1/shadow/name/State/update",
                                        "payload": {
                                            "contentExpression": "('{\"state\": {\"desired\": {\"far_alarm\": 2}}}')",
                                            "type": "JSON"
                                        }
                                    }
                                },
                                {
                                    "iotTopicPublish": {
                                        "mqttTopic": "$aws/things/Worker_Watch_2/shadow/name/State/update",
                                        "payload": {
                                            "contentExpression": "('{\"state\": {\"desired\": {\"far_alarm\": 2}}}')",
                                            "type": "JSON"
                                        }
                                    }
                                }
                            ]
                        },
                        {
                            "eventName": "ResetAlarmCounter",
                            "condition": "true",
                            "actions": [
                                {
                                    "setVariable": {
                                        "variableName": "AlarmCounter",
                                        "value": "5"
                                    }
                                }
                            ]
                        }
                    ]
                },
                "onExit": {
                    "events": []
                }
            }
        ],
        "initialStateName": "Offline"
    },
    "detectorModelDescription": null,
    "detectorModelName": "Connected_Worker_Gas_Alarm_Model",
    "evaluationMethod": "BATCH",
    "key": null
}
```

Now we can upload this template.  Once uploaded, it will take you to the edit screen.  The model is not saved and active until you click **Publish**, so let's click it.  
### Note:
If you are working with a teammate on this workflow, wait until they have finished their portion, otherwise you will receive an error.
![Step 9](/static/iotevents/9.png)

### Step 8:
Enter a name for the model, e.g. **Connected_Worker_Gas_Alarm_Model**.
Give it a role to assume to execute the model.  
As we do not have one premade, the service will create it for you with the necessary permissions.  You can give it the name **IoTEventsAlarmModelRole**.
We want to have a single detector, so that all devices send their data to the same model and can influence each other, i.e. change the alarm state of each other.
![Step 10](/static/iotevents/10.png)

Done!  We now have a working model and data inputs.
![Step 11](/static/iotevents/11.png)

### Step 9 (Optional):
If you click on the model, you can see the state it currently is in.  We can revisit this later and see how it changes.  As we are not forwarding data to it yet, it should show up as **offline**.
![Step 12](/static/iotevents/12.png)
