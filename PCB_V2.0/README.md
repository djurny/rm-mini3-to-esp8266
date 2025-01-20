# How to repurpose the IR blaster/receiver from a RM Mini 3
## Open the IR blaster
1. Use a knife to remove the top cover by tracing the gap between the top cover and the can.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/01%20open%20up%20the%20top.jpg" width="400"><br>
1. Use a flat head screwdriver to pry out the two PCBs from the can.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/02%20remove%20pcbs.jpg" width="400"><br>
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/03%20remove%20bottom%20weight.jpg" width="400"><br>
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/04%20bottom%20weight%20removed.jpg" width="400"><br>
## Disassemble the PCBs
1. Desolder the connections from the two PCBs.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/05a%20top%20view%20ir%20pcb.jpg" width="400"><br>
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/05b%20bottom%20view%20ir%20pcb.jpg" width="400"><br>
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/06a%20bottom%20view%20ir%20pcb.jpg" width="400"><br>
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/06b%20top%20view%20ir%20pcb.jpg" width="400"><br>
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/06c%20bottom%20control%20pcb.jpg" width="400"><br>
1. Locate the pads and their function.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/07%20bottom%20ir%20pcb%20pad-out.jpg" width="400"><br>
## Solder
1. Solder wires to the pads of the IR board and make sure to join the opposite pads.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/08%20test%20setup%20esp8266.jpg" width="400"><br>
1. Solder wires to your ESP device of choice:

    |Function|Pad|ESP8266 D1 mini ![ESP8266](https://i0.wp.com/randomnerdtutorials.com/wp-content/uploads/2019/05/ESP8266-WeMos-D1-Mini-pinout-gpio-pin.png?w=715&quality=100&strip=all&ssl=1)|ESP32 D1 mini ![ESP32 D1 mini](https://www.espboards.dev/img/sLgUCWcPxA-1000.png)|
    |-       |-  |-      |-            |
    |3V3     |1  |3V3    |3V3          |
    |GND     |2  |G      |GND          |
    |5V      |3  |5V     |VCC          |
    |RX      |4  |GPIO4 (D2)|GPIO16 (U2-RX)|    
    |TX      |5  |GPIO5 (D1)|GPIO17 (U2-TX)|

## ESPhome configuration
1. Add the `remote_receiver` and `remote_transmitter` to your ESPhome configuration.
    ```
    remote_transmitter:
    - id: IR_TX
      pin:
        number: GPIO17 # UART2-TX on ESP32 D1 MINI
        #       GPIO5  # D1 on ESP8226 D1 mini
        inverted: false
        mode:
          output: true
      carrier_duty_percent: 50%
    remote_receiver:
    - id: IR_RX
      pin:
        number: GPIO16 # UART2-RX on ESP32 D1 MINI
        #       GPIO4  # D2 on ESP8226 D1 mini
        inverted: true
        mode:
          input: true
          pullup: true
      dump:
        - all
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
## Print the parts using a 3D printer
1. The ESP32 bracket: [STL](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/STL/ESP32%20D1%20mini%20tray.stl)
1. The IR board riser:
    1. When you decide to keep the weight in the can, use the 36mm version: [STL](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/STL/IR%20board%20support%2036mm.stl)
    1. When you take out the weight (ideal when you want to put the can on top of a wall charger), use the 39mm version: [STL](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/STL/IR%20board%20support%2039mm.stl)
1. The IR board bracket: [STL](https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/STL/IR%20board%20holder.stl)
## Putting it back together (ESP32 D1 mini)
1. Make sure the can is empty.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/10%20soldered%20esp%20to%20ir%20board.jpg" width="400"><br>
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/11%20empty%20base.jpg" width="400"><br>
1. Wiggle the ESP32 from inside the can through the hole to the outside.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/12%20esp%20through%20base.jpg" width="400"><br>
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/13%20esp%20through%20base.jpg" width="400"><br>
1. Put the ESP32 in the bracket.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/14%20esp%20on%20tray.jpg" width="400"><br>
1. Slide the ESP32 bracket into the can. There is only one spot where the bracket can slide through. Slightly angled and through the center of the hole on the side of can.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/15%20slide%20tray%20into%20base.jpg" width="400"><br>
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/16%20snap%20tray%20into%20base.jpg" width="400"><br>
1. Put the IR board riser in the can.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/17%20put%20ir%20board%20bracket%20into%20base.jpg" width="400"><br>
1. Make sure the board support 'feet' are touching evenly.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/18%20put%20ir%20board%20bracket%20into%20base.jpg" width="400"><br>
1. Put the IR board bracket on top of the riser.
    There is an opening in the board holder. The outside circumference is slightly larger than the inside of the can, this is to make it fit snugly.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/19%20put%20ir%20board%20support%20onto%20bracket.jpg" width="400"><br>
1. While taking care of the wiring, place the IR board on top of the IR board bracket.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/20%20place%20ir%20board.jpg" width="400"><br>
1. Make sure that the IR LED that you most likely had to lift up to desolder the board, is pushed back down. Otherwise the top will not fit.
1. Put the top cover back on.
    <br><img src="https://github.com/djurny/rm-mini3-to-esp8266/blob/master/PCB_V2.0/Pictures/21%20put%20the%20top%20on.jpg" width="400"><br>
1. Power it up!

References:
- https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/
- [J3Y transistor](https://www.alldatasheet.net/datasheet-pdf/marking/226239/BILIN/S8050.html)
- [AMS1117 voltage regulator](https://datasheetgo.com/wp-content/uploads/2018/09/AMS1117-datasheet-pinout.gif)
- [Sensus IR & RF Code Converter](https://pasthev.github.io/sensus/)
