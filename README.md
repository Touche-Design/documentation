# Documentation
Connecting a Tactio node to a PC requires an intermediary device to bridge the CAN Tactio network to a common PC interface like USB. This walkthrough will show how to assemble such a system on a breadboard with an mbed LPC1768 as an intermediary along with a CAN transceiver.

## Required materials:
1. 1x Tactio node
2. 1x MCP2551 CAN transceiver breakout board
3. 1x mbed LPC1768
4. 1x USB-A to USB-Mini cable
5. 1x breadboard
6. jumper wires
7. 1x 120 ohm resistor

Note: We are only connecting 1 Tactio node for now, so we will power the system with our PC from the USB connection. However, if more nodes are being connected at the same time, a separate 5V power supply might be required. If this is the case, common grounds will be necessary.

## Wiring
With the LPC and the CAN transceiver on the breadboard, connect the devices together according to the following table:

|  LPC1768   | MCP2551 |
|------------|---------|
| 5V USB Out |   VCC   |
|    GND     |   GND   |
|   CAN TD   |   CTX   |
|   CAN RD   |   CRX   |

Next, we need to connect the Tactio node to the system with the pinout below. The CANH line on the node is connected to the CANH line on the transceiver, the CANL likewise is connected. Lastly, GND for the node is connected to the sytem's ground line, and the VCC connection is connected to the main power line, be it the 5V USB Out from the LPC, or an external power supply.

![Tactio node pinout](pinout.png)