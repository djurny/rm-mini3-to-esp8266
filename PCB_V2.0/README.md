# How to repurpose the IR blaster/receiver from a Broadlink RM Mini 3
## Open the IR blaster
1. Remove the top cover
    1. Use a knife to trace the gap between the top cover and the body.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/01%20open%20up%20the%20top.jpg)
    1. The top should come off after prying the gap all around.
1. Use a flat head screwdriver to pry out the two PCBs from the body.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/02%20remove%20pcbs.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/03%20remove%20bottom%20weight.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/04%20bottom%20weight%20removed.jpg)
## Disassemble the PCBs
1. Desolder the connections.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/05a%20top%20view%20ir%20pcb.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/05b%20bottom%20view%20ir%20pcb.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/06a%20bottom%20view%20ir%20pcb.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/06b%20top%20view%20ir%20pcb.jpg)
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/06c%20bottom%20control%20pcb.jpg)
1. Identify the pad functions.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/07%20bottom%20ir%20pcb%20pad-out.jpg)
## Solder
1. Solder wires, make sure to join the opposite pads.
    ![](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/pics/08%20test%20setup%20esp8266.jpg)
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
        pin: GPIO5
        carrier_duty_percent: 50%
    remote_receiver:
      - id: IR_RX
        pin: GPIO4
        dump: all
    ```

References:
- https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/
