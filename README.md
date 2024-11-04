# Getting started with Pepper

### General Info

Pepper has a chest button (under the tablet) which serves multiple functions:

| Press             | Function                                  |
|-------------------|-------------------------------------------|
| Once (Pepper off) | Turn Pepper on. It takes a while to boot. |
| Once (Pepper on)  | Pepper tells you its IP address.          |
| Long (3 Seconds)  | Turn Pepper off.                          |
| Twice | Switch the Autonomous Life mode on/off.         |

More details can be found in the documentation: http://doc.aldebaran.com/2-5/home_pepper.html. 

### Network Configuration

Pepper is configured to be in the local WiFi (NOWSZR2P).
The password is on the back of the router (in the lab).
To work with Pepper, you need to connect to the same WiFi. 
Note that it does not have internet access, so you might want to use another device to read documentation, or you could use a USB WiFi Adapter that allows you to connect to two networks at the same time.


### Programming Pepper

Pepper can be programmed with Python 2.7 using its SDK.
This is a combination of NAOqi and qi Framework, both of which are documented [here](http://doc.aldebaran.com/2-5/index_dev_guide.html). (TODO: check if Python 3 works as well. The qi package on PyPi does seem to support Python 3.)

There is also a ROS2 bridge that a previous student developed.
It only offers an adapter for certain motion commands and is not described here yet.

#### Setting up your environment

It is best practice to use a virtual environment.
I usually use [miniconda](https://docs.anaconda.com/miniconda/) for this, but you can use whichever.
```
conda create -n pepper_env python=2.7
conda activate pepper_env
pip install qi
```

This installs the qi package, which is all you need to program Pepper.

#### Usage

The general principle is as follows:
- Create a qi Session.
- Connect to the robot (use chest button to find out IP address).
- The qi Session object can be used to get individual services. These are the services from the [NAOqi API](http://doc.aldebaran.com/2-5/naoqi/index.html).
- Once you have a variable pointing to the service, you can use it as described in the NAOqi API.

Below, you find some examples.

```
import qi

# create session and connect to the robot
session = qi.Session()
session.connect('tcp://192.168.0.13:9559')

# use the Text to Speech service
tts = session.service('ALTextToSpeech')
tts.say('Hello, my name is Pepper!')

# use the Robot Posture service
ps = session.service('ALRobotPosture')
ps.goToPosture('StandInit', 0.2)
ps.goToPosture('StandZero', 0.2)
ps.goToPosture('Crouch', 0.2)
```

Note that the NAOqi API is developed for a number of robots, not just Pepper.
Our Pepper might not support all the behaviours that are documented.


### Troubleshooting / Issues

The Pepper in our lab is version 1.6 and has NAOqi version 2.5.5.5 installed.

Pepper's right arm does not always do what it's supposed to do.
It used to be broken, then went to repair, but it does not seem to be fully fixed. Sometimes it works fine, sometimes it does not.

The tablet seems to be frozen. 
We might need to do a factory reset to get it going again.
However, the interface can be accessed via browser when connected to the WiFi or by cable.

To access the interface, put pepper's IP address as URL into the browser.
If direct access by cable is required: 
- turn pepper off
- open head, connect ethernet cable directly from laptop to head
- turn pepper on
- press pepper's chest button once, it says its IP address
- put this as URL into your browser
- access web interface (needs username/password)
