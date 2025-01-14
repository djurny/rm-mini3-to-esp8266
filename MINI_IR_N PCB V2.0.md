1. Remove the top cover
    1. Use a knife to trace the gap between the top cover and the body.
    1. The top should come off after prying the gap all around.
1. Use a flat head screwdriver to pry out the two PCBs from the body.
1. Desolder the connections.
1. Identify the pad functions.
1. Solder wires, make sure to join the opposite pads.
1. Solder wires to your ESP device:

    |Function|Pad|ESP|
    |-       |-  |-  |
    |3V3     |1  |3V3|
    |GND     |2  |G  |
    |5V      |3  |5V |
    |RX      |4  |GPIO4 (D2)|
    |TX      |5  |GPIO5 (D1)|

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


