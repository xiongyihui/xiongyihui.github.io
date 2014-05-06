---
layout: post
title: "Capacitive Touch"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Capacitive touch is widely used in our dairly life. Curious about how it works?
Let's build a capacitive button based on [mbed platform](http://mbed.org).

![Capacitive Touch](/assets/images/capacitive_touch.jpg)

The capacitance of a capacitive button will be changed when with a human touch.
There are several ways to measure the change of the capacitance. Here we use the
charging/discharging time of a simple RC circuit to correlate the capacitance.

## Hardware
+ An mbed board (Arch is used here)
+ An apple with a bite
+ A wire

Use the wire to connect the Arch's P0_11 with the apple.

## Software
The pull-down feature of a general porpuse I/O is used to discharging the 
capacitor of the touch button. A Ticker is used to measure the discharging time 
of the capacitor every 1/64 seconds.

*Capacitive Touch Program* - [Import program to mbed online compiler](https://mbed.org/compiler/#import:/users/yihui/code/Arch_Capacitive_Touch/;platform:Seeeduino-Arch)

~~~{.cpp}
#include "mbed.h"
 
DigitalOut led(LED1);
DigitalInOut touch(P0_11);       // Connect a wire to P0_11
Ticker tick;
 
uint8_t touch_data = 0;         // data pool
 
void detect(void)
{
    uint8_t count = 0;
    touch.input();              // discharge the capacitor
    while (touch.read()) {
        count++;
        if (count > 4) {
            break;
        }
    }
    touch.output();
    touch.write(1);             // charge the capacitor
    
    if (count > 2) {
        touch_data = (touch_data << 1) + 1;
    } else {
        touch_data = (touch_data << 1);
    }
    
    if (touch_data == 0x01) {
        led = 1;                // touch
    } else if (touch_data == 0x80) {
        led = 0;                // release
    }
}
 
int main()
{
    touch.mode(PullDown);
    touch.output();
    touch.write(1);
    
    tick.attach(detect, 1.0 / 64.0);
    
    while(1) {
        // do something
    }
}
~~~

