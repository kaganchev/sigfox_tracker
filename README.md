# sigfox_examples
Custom made GPS tracker and accelerometer data examples for low-powered Sigfox module TD1205P.

## Used information

* Telecom Design SDK given for their Sigfox modules - https://github.com/Telecom-Design
* Basics and message formating from this tutorial - http://www.instructables.com/id/Sigfox-GPS-Tracker/

## General Message Fromat
  In all of the examples is used 12 bytes message format for transceiving the information from the module(GPS and accelerometer data). Message is formed by using some of the code in above tutorial:
  4 bytes - GPS latitude
  4 bytes - GPS longitude
  1 byte - Voltage
  1 byte - Temperature
  2 bytes - Free or used to tranceive the minimum value of one of the accelerometer axis  

## Example acc_calc.c
 Calculates and prints minimum, maximum, the average and standard deviation of the accelerometer data for x, y, z at given interval
### Commands
#### AT$ACCFREQ= 
- If empty prints the current value
- (value: frequency) - (1: 1Hz), (2: 10Hz), (3: 25Hz), (4: 50Hz), (5: 100Hz), (6: 200Hz), (7: 400Hz), (8: 1.25KHz)
- Default value - 10Hz
#### AT$ACCSCALE=
- If empty prints the current value
- (value: scale) - (1: 2G), (2: 4G), (3, 8G), (4: 16G)
- Default value - 2G
#### AT$ACCDATA=
- 0: turn off the accelerometer
- 1: turn on the accelerometer
- 2: enable/disable printing of current accelerometer data
#### AT$CALC=
- 0: turn off the accelerometer and calculation of the data
- interval in seconds: turn on the accelerometer and calculate the data in some interval
### Baud rate - 460800 bps
The higher is the baud rate, the higher frequency can be set. If it is not enough, the program will give exception during printing the values from the accelerometer in the terminal.

## Example acc_data.c
Configures the accelerometer and prints 3 axes data
### Commands
#### AT$ACCFREQ= 
- If empty prints the current value
- (value: frequency) - (1: 1Hz), (2: 10Hz), (3: 25Hz), (4: 50Hz), (5: 100Hz), (6: 200Hz), (7: 400Hz), (8: 1.25KHz)
- Default value - 1Hz
#### AT$ACCSCALE=
- If empty prints the current value
- (value: scale) - (1: 2G), (2: 4G), (3, 8G), (4: 16G)
- Default value - 2G
#### AT$ACCDATA=
- 1: turn on/off the accelerometer
- 2: enable/disable printing of current accelerometer data
### Baud rate - 460800 bps
The higher is the baud rate, the higher frequency can be set. If it is not enough, the program will give exception during printing the values from the accelerometer in the terminal.

## Example pw_test.c
Configures GPS and sends messages at given interval
* Added custom commands for GPS timeout, GPS fixing interval and GPS fixing mode
* Defaults: 180 seconds timeout, 2 hours fixing interval, TD_GEOLOC_HW_BCKP GPS mode
* 12 bytes message - 4 bytes latitude, 4 bytes longitude, 1 byte voltage, 1 byte temperature
* Sends keepalive message at every boot
* At every boot waits 180 seconds for commands through the serial(if connected), after that starts fixing with default values.
### Commands
#### AT$MODE=
- sets GPS sleep mode
- (value: mode) - (0: TD_GEOLOC_OFF), (1: TD_GEOLOC_HW_BCKP), (2: TD_GEOLOC_POWER_SAVE_MODE)
#### AT$TIMEOUT=
- sets GPS timeout
- timeout in seconds
#### AT$PWTEST=
- manually starts the fixing with given interval
- fixing interaval in seconds
