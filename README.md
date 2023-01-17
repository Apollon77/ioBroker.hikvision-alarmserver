![Logo](admin/hikvision-alarmserver.png)
# ioBroker.hikvision-alarmserver

[![NPM version](https://img.shields.io/npm/v/iobroker.hikvision-alarmserver.svg)](https://www.npmjs.com/package/iobroker.hikvision-alarmserver)
[![Downloads](https://img.shields.io/npm/dm/iobroker.hikvision-alarmserver.svg)](https://www.npmjs.com/package/iobroker.hikvision-alarmserver)
![Number of Installations](https://iobroker.live/badges/hikvision-alarmserver-installed.svg)
![Current version in stable repository](https://iobroker.live/badges/hikvision-alarmserver-stable.svg)
[![Dependency Status](https://img.shields.io/david/raintonr/iobroker.hikvision-alarmserver.svg)](https://david-dm.org/raintonr/iobroker.hikvision-alarmserver)

[![NPM](https://nodei.co/npm/iobroker.hikvision-alarmserver.png?downloads=true)](https://nodei.co/npm/iobroker.hikvision-alarmserver/)

**Tests:** ![Test and Release](https://github.com/iobroker-community-adapters/ioBroker.hikvision-alarmserver/workflows/Test%20and%20Release/badge.svg)

## Hikvision Alarm Server adapter for ioBroker

An adapter to receive alarms/events sent from Hikvision cameras.

Tested with Hikvision models:

- DS-2CD2043G2-I
- DS-2CD2143G2-I
- DS-2DE2A404IW-DE3
- DS-2DE3A404IW-DE/W

Success/failure/bug reports welcomed if you have a model not in this list.

## Usage

The adapter instance creates a boolean state for each combination of camera/event type reported. Cameras are identified by MAC address (limited by information given by camera).

It appears that cameras repeatedly issue events every second when those events are still valid but no message is sent to clear them. For this reason the adapter automatically clears events that have not been re-reported for more than 5 seconds.

## Configuration

### ioBroker

#### Network

In the adapter configuration, select a free port for the adapter to listen on (8089 by default).

#### Alarm timeout

Most devices signal an alarm is *active* by constantly sending alert messages. These devices never send an *inactive* message. Therefore the adapter assumes an alarm is cleared when no message is received after a given period of time. Specify that period here (default 5000ms).

#### Channel tree

Some cameras (eg. with multiple sensors) report on multiple channels (not to be confused with ioBroker channels). In order to differentiate events between each of the camera's channels check the appropriate option.

For specific event types (eg. field detection, line crossing, etc), some cameras are able to identify motion detection targets (eg. human, vehicle, etc). To create a state for each of these targets under each applicable event type, check the appropriate option.

#### sendTo

Some event types received have a simple boolean on/off (duration, VMD, etc). For these simple events, setting the appropriate state in ioBroker's object tree is sufficient.

However, some events received include binary data such as images which would be impractical to constantly store in the ioBroker object tree. A more graceful mechanism to handle such events it to use ioBroker's inbuilt messaging system which allows message objects to be communicated between adapters.

Configuration in this section can cause event data (configurable in the `Send to message` field) to be sent to the appropriate adapter.

A simple example would be if the Telegram adapter has been installed, one could set the following parameters:

* Send to instance name: `telegram.0`
* Send to command: Leave blank
* Send to message: `{ text: imageBuffer, type: 'photo' }`

With this, each image received is sent on to the telegram adapter for direct distrubtion to users.

Using a modified configuration, images could be sent to email for example, or to a custom script (using Javascript adapter) for further processing.

#### Saving event data

If checked event XML and/or image data is store on the local filesystem under `iobroker-data/hikvision-alarmserver.<instance>`.

*Warning!* these files are not currently purged or archived so use with caution or implement an external strategy for this.


### On Camera

Visit the configuration page of your camera(s) and define ioBroker IP/host and port settings:

![Alarm Server Options](docs/images/alarm-server-options.png)

Make sure to linkage in the events you would like to report to ioBroker includes 'Notify Surveillance Center'. Eg:

![Motion Detection Options](docs/images/motion-detection-options.png)

## Changelog

<!--
  Placeholder for the next version (at the beginning of the line):
  ### **WORK IN PROGRESS**
-->
### **WORK IN PROGRESS**
-   (Robin Rainton) Added configuration for alarm timeout ([#16](https://github.com/iobroker-community-adapters/ioBroker.hikvision-alarmserver/issues/16)).
-   (Robin Rainton) Fixed multipart message handling for line crossing/field detection, etc ([#18](https://github.com/iobroker-community-adapters/ioBroker.hikvision-alarmserver/issues/18)).
-   (Robin Rainton) Optionally save XML/images & send events using `sendTo` to other adapters ([#20](https://github.com/iobroker-community-adapters/ioBroker.hikvision-alarmserver/issues/20)).

### 0.0.7 (2022-12-29)
-   (Robin Rainton) Add bind address option ([#9](https://github.com/iobroker-community-adapters/ioBroker.hikvision-alarmserver/issues/9)).
-   (Robin Rainton) Try to derive device names from net-tools. Optionally use channelName from devices ([#10](https://github.com/iobroker-community-adapters/ioBroker.hikvision-alarmserver/issues/10)).

### 0.0.6 (2022-12-13)
-   (Robin Rainton) Handle multipart message payload ([#5](https://github.com/iobroker-community-adapters/ioBroker.hikvision-alarmserver/issues/5)).
-   (Robin Rainton) Handle payloads without XML declaration ([#7](https://github.com/iobroker-community-adapters/ioBroker.hikvision-alarmserver/issues/7).)

### 0.0.5 (2022-12-10)
-   (Robin Rainton) Drop colons from device IDs.

### 0.0.2
-   (Robin Rainton) initial release.

## License
MIT License

Copyright (c) 2022 Robin Rainton <robin@rainton.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.