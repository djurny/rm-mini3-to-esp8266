# How to repurpose the IR blaster/receiver from a RM Mini 3
## Open the IR blaster
1. Remove the top cover
    1. Use a knife to trace the gap between the top cover and the body.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/01%20open%20up%20the%20top.jpg)
    1. The top should come off after prying the gap all around.
1. Use a flat head screwdriver to pry out the two PCBs from the body.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/02%20remove%20pcbs.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/03%20remove%20bottom%20weight.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/04%20bottom%20weight%20removed.jpg)
## Disassemble the PCBs
1. Desolder the connections.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/05a%20top%20view%20ir%20pcb.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/05b%20bottom%20view%20ir%20pcb.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/06a%20bottom%20view%20ir%20pcb.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/06b%20top%20view%20ir%20pcb.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/06c%20bottom%20control%20pcb.jpg)
1. Identify the pad functions.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/07%20bottom%20ir%20pcb%20pad-out.jpg)
## Solder
1. Solder wires, make sure to join the opposite pads.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/08%20test%20setup%20esp8266.jpg)
1. Solder wires to your ESP device:

    |Function|Pad|ESP|
    |-       |-  |-  |
    |3V3     |1  |3V3|
    |GND     |2  |G  |
    |5V      |3  |5V |
    |RX      |4  |GPIO4 (D2)|
    |TX      |5  |GPIO5 (D1)|

## ESPhome configuration
1. ESPhome configuration
    ```
    remote_transmitter:
      - id: IR_TX
        pin:
          number: GPIO5
          inverted: false
          mode:
            output: true
            open_drain: false
        carrier_duty_percent: 50%
    
    remote_receiver:
      - id: IR_RX
        pin:
          number: GPIO4
          inverted: true
          mode:
            input: true
        dump: all
    ```
1. ESPhome logging showing sample dump from random remote control
    ```
    [17:06:40][VV][remote_receiver.esp8266:107]:   i=20 buffer[49]=45382431 - buffer[48]=45381515 -> -916
    [17:06:40][I][remote.pronto:234]: Received Pronto: data=
    [17:06:40][I][remote.pronto:236]: 0000 006D 000B 0000 0046 0042 0024 0020 0046 0020 0024 0020 0024 0020 0024 0020 0024 0043 0046 0042 0024 0021 0024 0021 0024 0180 06C3
    [17:06:40][I][remote.rc5:086]: Received RC5: address=0x10, command=0x57
    [17:06:40][W][component:237]: Component remote_receiver took a long time for an operation (223 ms).
    [17:06:40][W][component:238]: Components should block for at most 30 ms.
    [17:06:41][VV][scheduler:225]: Running interval '' with interval=10000 last_execution=36283 (now=46283)
    ```
    
References:
- https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/
- [J3Y transistor](https://www.alldatasheet.net/datasheet-pdf/marking/226239/BILIN/S8050.html)
- [AMS1117 voltage regulator](https://datasheetgo.com/wp-content/uploads/2018/09/AMS1117-datasheet-pinout.gif)
- [Sensus IR & RF Code Converter](https://pasthev.github.io/sensus/)
