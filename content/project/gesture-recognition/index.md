---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Contactless Gesture Recognition"
summary: "A contactless gesture recognition system that uses IR Proximity Sensors to classify different hand gestures. Interfaced with VLC Media player to pause and play videos."
authors: [Archana Swaminathan]
tags: [Electronics]
categories: [Undergraduate Projects]
date: 2020-08-16T11:03:12+05:30

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

url_code: "https://github.com/archana1998/Contactless-Gesture-Recognition-System-"
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

# Introduction

Gesture Recognition Systems are commonly utilized as an interface between computers and humans, along with interacting with many electronic instruments. These systems can be classified into three classes as follows:
1. Motion-based: When the user holds a device or a controller that detects the gesture made.
2. Touch-based: When the system includes a touch-screen and the positions and directions of the finger or equivalent tool of the user are mapped, thus recognizing the gesture.
3. Vision-based: When the system makes use of image and signal processing to detect gestures made without touching any device.


The first two types of systems need the users to hold and contact certain devices, for the gesture recognition, and vision-based systems use camera setups, image processing and techniques that involve computer vision. These systems are difficult to set up for small scale use and are also expensive and extremely power hungry. For building a system that needs to function when there are limited resources available, it is important that the setup cost, power consumption and ease and size of setup is taken into consideration. Keeping this in mind, we have built a contactless gesture recognition system that consists of a couple of digital infrared sensors, that have been programmed to do the gesture recognition using a custom algorithm, with an Arduino Uno Microcontroller.

# Problem Solving Methodology

Components Used and Setup:
The components we used are:
    - Breadboard
    - Arduino Uno Microcontroller
    - 2 digital IR Sensors
    - Jumper wires
    - Laptop for interfacing
Languages used:
    - Arduino IDE (Based on C++)
    - Python (for interfacing sensor output with VLC)

We connected two IR sensors to the Breadboard, placed at a distance of approximately 3 cm from each other. These were then interfaced with the Arduino Uno, which was connected to the laptop. 

Voltage is applied to the pair of IR LEDs, which in succession emit Infrared light. This light propagates through the air and once it hits the hand (or object), which acts as a hurdle, it is reflected back to the receiver. The LED on the diode glows, thus indicating that an object has been detected.

A digital sensor system consists of the sensor itself, a cable, and a transmitter. The sensor has an electronic chip. The measuring signal is directly converted into a digital signal inside the sensor. The data transmission through the cable is also digital. This digital data transmission is not sensitive to cable length, cable resistance or impedance. 

Using the concept of states and delay as in Digital Design, we have created two states of the two sensors each placed on the left and right. These two states are defined and calibrated using a time delay of a few hundred microseconds in the gesture classification algorithm written using the Arduino IDE. 

We have taken two states in the algorithm into consideration namely Q(t) and Q(t+d) where d is the delay defined. The algorithm is defined such that left sensor and right sensor digital values are checked first and then after the defined delay, both sensors are checked for their Boolean values again and therefore the gesture is recognized and printed on the screen after running the code in the Arduino software. The chip on the Arduino Uno board plugs straight into the laptop’s USB port and supports the computer as a virtual serial port.

To make the gesture recognition feature interactive, we have interfaced the output with VLC Media player, so that we can pause, play and rewind/go forward with the playback. To do this interfacing, we have written a Python script, importing the Python library pyautogui, that provides functionality of control of the computer’s keyboard.


{{< figure src="setup.jpg" title="Setup" numbered="true" lightbox="false" >}}


# Results and Conclusions

We tested the gesture recognition system for accuracy by using the precision-recall matric. We took in 30 different samples for input.
The precision is calculated as TP/(TP+FP), where TP denotes the number of true positives and FP denotes the number of false positives.
The precision of our system is = 90% (27 TPs and 3 FPs)

The recall is calculated as TP/(TP+FN), where TP denotes the number of true positives, and FN denotes the number of false negatives.
The recall of our system is = 86.6% (26 TPs and 4 FNs)

High precision implies that there is less chances of getting false alarms, and recall expresses the ability to find all relevant information from the dataset.
Practical Applications
We have integrated the recognition system with VLC, to control playback of the video. Similarly, the setup can easily be integrated with any mobile device that is low on resources, as well as used with complex devices, as IR sensors are fundamental in building light reflection systems and are extremely versatile.


# Further scope

    - Friendly user interface that can be easily understood by any user and eventually its application can be extended to more applications like PDF reader, video games etc.
    - Computationally inexpensive and low power consuming hardware and software setup, that makes it ideal for integrating with any device, both simple and complex.
                                     
# Limitations

    - Ambient light obstructs the functioning as is the case with infrared sensors, as they are extremely sensitive. A proper optical barrier must be used to prevent this.
    - We have assumed values of time delays between gestures according to what worked well for our test dataset. This leads to the system being slightly inflexible with different speeds of gestures. 






