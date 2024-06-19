+++
title = 'Testing push notifications locally in an iOS app'
date = 2024-06-19T07:13:19+03:00
tags = ["Testing", "Push Notifications", "locally", "APNS", "RocketSim", "iOS", "app"]
draft = false
+++

### Introduction
I always wondered how I could automate testing the push notification process. Even when Apple introduced the possibility of dragging a configured file to the simulator to display a notification, it is still a manual process. I'll skip testing via the terminal because I think it takes more time than using an APNS file or the [RocketSim app](https://www.rocketsim.app/).

Before I was first introduced to the RocketSim app, I used an APNS file for testing push notifications. It worked for me and my teammates, but I knew it could be better. It looks something like this:

### Testing Using an APNS File
To test push notifications using an APNS file, you need to create a file with a `.apns` extension and put JSON into it.

```json
{
  "Simulator Target Bundle": "dev.mt.Demo",
  "aps": {
    "alert": {
      "title": "Title",
      "body": "Body"
    }
  }
}
```

You also need to specify the `Simulator Target Bundle`; without it, you will receive an error.
![alt image](images/0.png#center)

After that, you can drag this file to the simulator.
![alt image](images/1.gif#center)

When I found the RocketSim app and learned about its ability to send push notifications from the simulator side menu, it was a game changer for me. No more dragging files or terminal commands—it’s all in one place. The only thing you need to do is create the payload.

### Testing Using the RocketSim App
To test push notifications using the RocketSim app, all you need to do is:

1. Open the RocketSim app.
2. Go to the simulator side menu.
3. Click on the bell button.
4. Click on the “Configure Push Notifications” button.

![alt image](images/2.png#center)

The last step is to add groups, set the bundle identifier, switch to push notifications, and add a push notification. 
![alt image](images/5.png#center)
![alt image](images/4.png#center)

Now you have configured the push notification, and from now on, you can send it with just a tap in the RocketSim app's simulator side menu.
![alt image](images/6.png#center)

### Summary
Using the RocketSim app saved me a lot of time and effort. Without it, I would still be testing manually and spending time on work that could be automated. I would recommend this tool to anyone.

#### Thank you for reading! 😊
