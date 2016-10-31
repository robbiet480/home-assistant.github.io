---
layout: page
title: "Device Tracker"
description: "Instructions how to setup device tracking within Home Assistant."
date: 2015-01-20 22:36
sidebar: true
comments: false
sharing: true
footer: true
---

Home Assistant can get information from your wireless router or third party services like iCloud or OwnTracks to track which devices are connected and considered "in home". Please check the sidebar for a list of  brands of supported wireless routers and services.

There are also trackers available which use different technologies like [MQTT](/components/mqtt/) or [Nmap](/components/device_tracker.nmap_tracker/) to scan the network for devices.

# {% linkable_title Configuring a `device_tracker` platform %}

To get started add the following lines to your `configuration.yaml` (example for Netgear):

```yaml
# Example configuration.yaml entry for Netgear device
device_tracker:
  - platform: netgear
    host: 192.168.1.1
    username: admin
    password: YOUR_PASSWORD
```

The following optional parameters can be used with any platform. However device tracker will only look for global settings under the configuration of the first configured platform:

| Parameter           | Default | Description                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `track_new_devices` | True    | If new discovered devices are tracked by default                                                                                                                                                                                                                                                                                                                                          |
| `interval_seconds`  | 12      | Seconds between each scan for new devices                                                                                                                                                                                                                                                                                                                                                 |
| `consider_home`     | 180     | Seconds to wait till marking someone as not home after not being seen. This parameter is most useful for households with Apple iOS devices that go into sleep mode while still at home to conserve battery life. iPhones will occasionally drop off the network and then re-appear. `consider_home` helps prevent false alarms in presence detection when using IP scanners such as Nmap. `consider_home` accepts various time representations, (E.g. the following all represents 3 minutes: `180`, `0:03`, `0:03:00`)  |

The extended example from above would look like the following sample:

```yaml
# Example configuration.yaml entry for Netgear device
device_tracker:
  - platform: netgear
    host: 192.168.1.1
    username: admin
    interval_seconds: 10
    consider_home: 180
    track_new_devices: yes
```

Multiple device trackers can be used in parallel, such as [Owntracks](/components/device_tracker.owntracks/) and [Nmap](/components/device_tracker.nmap_tracker/). The state of the device will be determined by the source that reported last.

# {% linkable_title `known_devices.yaml` %}

Once `device_tracker` is enabled, a file will be created in your config dir named `known_devices.yaml`. Edit this file to adjust which devices to be tracked.

Here's an example configuration for a single device:

```yaml
devicename:
  name: Friendly Name
  mac: EA:AA:55:E7:C6:94
  icon:  mdi:face-profile
  picture: https://home-assistant.io/images/favicon-192x192.png
  gravatar: test@example.com
  track: yes
  hide_if_away: no
```

| Parameter      | Default                       | Description                                                                                             |
|----------------|-------------------------------|---------------------------------------------------------------------------------------------------------|
| `name`         | Host name or "Unnamed Device" | The friendly name of the device                                                                         |
| `mac`          | None                          | The MAC address of the device. Add this if you are using a network device tracker like Nmap or SNMP     |
| `icon`         | None                          | A name of an MDI Icon.                                                                                  |
| `picture`      | None                          | A picture that you can use to easily identify the person or device. If provided, will override 'icon'. You can also save the image file in a folder "www" in the same location (can be obtained from developer tools) where you have your configuration.yaml file and just use `picture: /local/favicon-192x192.png`. |
| `gravatar`     | None                          | An email address for the device's owner. If provided, it will override `picture`                        |
| `track`        | False                         | If  `yes`/`on`/`true` then the device will be tracked. Otherwise its location and state will not update |
| `hide_if_away` | False                         | If `yes`/`on`/`true` then the device will be hidden if it is not at home                                |
| `consider_home` | [uses platform setting]      | Allows you to override the global `consider_home` setting from the platform configuration on a per device level                                |
