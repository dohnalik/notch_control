# Control API of a programmable notch filter for THD measurement

	get command returns push
	set command returns push to confirm new value 
	push command to notch is not a valid cmd, its only pushed back by Mini Streamer to return/confirm set value.

	UART/CDC is running at 115200kbps, no parity, 1 stop bit. 
            
#### SET NOTCH FREQUENCY

Sets the desired notch filter frequency.

```shell
[getNotchFreq]{}
```

```shell
[setNotchFreq]{notchFrequency:1005.42}
```

```shell
[pushNotchFreq]{notchFrequency:1005.42}
```
notchFrequency:

frequency in Hz, in range from 10.0 Hz to 200000.0 Hz, floating point

#### SET CAPACITOR DECADE

If manual capacitor set is requied can be set by this API call. 

```shell
[getCapacitors]{}
```

```shell
[setCapacitors]{decade:1}
```

```shell
[pushCapacitors]{decade:1}
```

decade:

decade in range from 0 (330pF) to 3 (333.33nF), integer
	
#### ADC CALIBRATION

```shell
[getADCcal]{port:1,type:0} - reads back calibration value with:
```
```shell
[pushADCcal]{adcCal:+-xxxxxxx}
```

```shell
[setADCcal]{port:1,type:0} - calibrates port
```

```shell
[pushADCcal]{confirmCalibrationStart}
```

```shell
[replyADCcal]{y} OR [replyADCcal]{n}
```
port:

	0   "USB1",	
	1   "USB2",	
  	2   "USB3",	
type:

	0   "OFFSET",	
	1   "RDSON",	only valid for USB2 and 3

Calibration will timeout after 5s without reply
PWR is self calibrated

#### ADC CALIBRATION - recall factory cal defaults

```shell
[getADCdefaults]{port:1} - reads out factory defaults
```

```shell
[setADCdefaults]{port:1} - sets back factory defaults for specific port
```

```shell
[pushADCdefaults]{offset:xxxx,senseR:80} 
``` 

RDSon is sent back only for USB 2 and 3


#### ADC CALIBRATION - set factory cal defaults 
##### DONT USE, FOR PRODUCTION PURPOSES ONLY!!!

```shell
[setADCfactory]{I know what I'm doing}
```

```shell
[pushADCfactory]{confirmCalibrationDefaults}
```

```shell
[replyADCfactory]{y} OR [replyADCfactory]{n}
```

Calling setADCfactory will load current calibration profile for all ports into internal memory as factory defaults.

#### POWER/UNIT STATUS

```shell
[getPowerStatus]{}
```

```shell
[pushPowerStatus]{status:0}
```

status:

	0   Everything is OK,	
	1   Input voltage too low, audio performance could be degraded, connect 18V power supply
  	2   Unit failure, input regulator could be damaged
  	
	
#### Mini Streamer HW BOOT CONTROL

```shell
[pushBoot]{boot:0}
```

```shell
[setBoot]{boot:0}
```

boot:

	0   note defined,	
	1   puts CM3 into USB bootloader mode where new Streamer Software image can be uploaded over USB	
	
Bootloader mode has to be switched off by user presing "USB" toggle button on the unit front panel

	
#### POWER OFF CONTROL

Is pushed from the Mini Streamer HW to Streamer Software to signal that user has switched off the unit with hardware front panel toggle. Streamer Software then needs to start shutting down. After its ready to cut the power off, it sets GPIO34 low and power is going to be cut 3 seconds later.

If GPIO34 is not put low within 60 seconds, the unit will be shut anyway. If GPIO34 is low when shutdown is commanded (shall go high when Streamer Software has booted up), unit is shut down immediately. 

```shell
[getTurnOff]{}
```

```shell
[setTurnOff]{cmd:0}
```

```shell
[pushTurnOff]{cmd:0}
```
cmd:

	0 - not valid
	1 - unit is shutting down (either set from Streamer Software, or pushed from the Mini Streamer HW)

#### Mini Streamer HW VERSION

```shell
[getBoardVersion]{}
```

```shell
[pushBoardVersion]{boardRev:12,firmwareRev:11}
```

#### STREAMER VERSION

```shell
[getStreamerVersion]{}
```

```shell
[pushStreamerVersion]{version:1223}
```

```shell
[setStreamerVersion]{version:1223}
```
#### TODO:

Define UART bootloader for onboard firmware upgrade
