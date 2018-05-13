# Control API of a programmable notch filter for THD measurement

	get command returns push
	set command returns push to confirm new value 
	push command to notch is not a valid cmd, its only pushed back to return/confirm set value.

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
	
#### SET RESISTOR LADDER

If manual resistor set is requied can be set by this API call. 

```shell
[getResistors]{}
```

```shell
[setResistors]{value:63}
```

```shell
[pushResistors]{value:63}
```

value:

ladder value, in range from 0 (160k) to 63 (1.25k), 6bit integer. 

#### SET DAC ARRAY

If manual multibit trim DAC set is requied can be set by this API call. 

```shell
[getDACs]{}
```

```shell
[setDACs]{value:4526}
```

```shell
[pushDACs]{value:4156}
```

value:
DAC value, 16bit integer. 

#### FW VERSION

```shell
[getBoardVersion]{}
```

```shell
[pushBoardVersion]{firmwareRev:11}
```

#### TODO:

Define UART bootloader for onboard firmware upgrade
