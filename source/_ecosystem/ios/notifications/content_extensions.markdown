---
layout: page
title: "Dynamic content"
description: "Extend your notifications with dynamic content"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
---

With the new Content Extension feature found in iOS 10, dynamic content can now be displayed as part of a notification without opening an app.

# Map
Will show a map with a red tipped pin at the coordinates given.
The map will be centered at the coordinates given.

```yaml
service: notify.iOSApp
data:
  message: Something happened at home!
  data:
    push:
      category: map
    action_data:
      latitude: 40.785091
      longitude: -73.968285
```

<p class='img'>
  <img src='/images/ios/map.png' />
  An example of the map dynamic content.
</p>


# Camera Stream

The notification thumbnail will be a still image from the camera.
The notification content is a real time MJPEG stream of a camera (assuming the camera supports it).

You can use the attachment parameters `content-type` and `hide-thumbnail` with camera.

You can view an example [here](https://www.youtube.com/watch?v=LmYwpxPKW0g).

```yaml
service: notify.iOSApp
data:
  message: Motion detected in the Living Room
  data:
    push:
      category: camera
    entity_id: camera.demo_camera
```

<div class='videoWrapper'>
<iframe width="560" height="315" src="https://www.youtube.com/embed/LmYwpxPKW0g" frameborder="0" allowfullscreen></iframe>
</div>

# Combining with actionable notifications

As you can see the `category` key is used to tell the device what kind of content extension to use. You can use the same category identifiers in your own custom [actions](http://localhost:4000/ecosystem/ios/notifications/actions/) to add actions to the content extension.
