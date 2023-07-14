# Brownout-detection-problem-in-Arduino-and-ESP32-C3
Brownout detection problem in Arduino and ESP32-C3

so, for a long time, I personally, had lots of problems with ESP32-C3. if you are using ESP-IDF, you wouldn't have this problem. but i am using Arduino. as i have seen in internet, many many other programmers have the same problem. RESETTING at the boot and repeat and repeat. sometimes after few times, it goes away. but it is always in the corner to sh!#t on your thoughts! so, what is this? and why and can it be fixed?
if you search about Brownout reset level in Arduino, you will see that they say it is in the lowest level (2.43 V) and you can not change it, sense it is hard coded in Arduino and if you want to change it, you should compile the Arduino SDK for the chip.
i am here to tell you, it is bull sh!#t ! and wrong! and YOU CAN FIX IT! 
i search the SDK for it and found out that the Brownout Reset is actually is set at level 7 and not 1 !!!! holy hell! but you do NOT need to compile the SDK to change it! 
so here it is:
HOW TO CHANGE THE Brownout LEVEL:
there are 4 files you have to change. and yes! they have all the things we all missing in Arduino. it has all the values for time outs and more.
i am currently using the last version (2.0.9). but it doesn't matter what version you will have. they are the same. remember, DO NOT REPLACE THE FILES, always search and replace what you want. 
all 4 files have the same name, but different addresses.
`sdkconfig.h`
`C:\Users\XXX\AppData\Local\Arduino15\packages\esp32\hardware\esp32\2.0.9\tools\sdk\esp32c3\dout_qspi\include`
`C:\Users\XXX\AppData\Local\Arduino15\packages\esp32\hardware\esp32\2.0.9\tools\sdk\esp32c3\qio_qspi\include`
`C:\Users\XXX\AppData\Local\Arduino15\packages\esp32\hardware\esp32\2.0.9\tools\sdk\esp32c3\qout_qspi\include`
`C:\Users\XXX\AppData\Local\Arduino15\packages\esp32\hardware\esp32\2.0.9\tools\sdk\esp32c3\dio_qspi\include`
the XXX is your user name.
the thing you need to change is this:
`#define CONFIG_ESP32C3_BROWNOUT_DET_LVL 7`
you can put what ever level you want. like this
`#define CONFIG_ESP32C3_BROWNOUT_DET_LVL 3`
or 1 if you want.
but you have to change all 4 files.
that's it! 
good luck!

-----------------
edit 1:
it appears I made few confusions! First, ESP32 and ESP32C3 are way different from each other. C3 has a very very high voltage trigger. E.g., Esp32 lowest trigger level is around 2.44 volt, but C3 is over 3 volts. So that is why C3 has a big problem.
Second:
CONFIG_ESP32C3_BROWNOUT_DET_LVL 7 is the highest level, meaning, the highest voltage needed. I think it is 3.2-volt trigger. It is a very happy trigger chip! And CONFIG_ESP32C3_BROWNOUT_DET_LVL 1 is around 3 (a bit more than 3) volt. If you use usb or 5 volts for input and give it to a low drop regulator, it is high possibility to reset the chip. If you want to be sure, do this:
Enable debug level info. Then try to connect to it via WIFI or do something that causes reset. Then when it resets, it prints in serial port, what caused the reset. If it is brownout reset, it prints it.

