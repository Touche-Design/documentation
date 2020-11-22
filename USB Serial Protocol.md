USB Serial Protocol
===================

The network controller firmware communicates with a PC over a simple USB serial protocol. This protocol is implemented by the PyTactio library, but it is documented below for creating separate implementations. All messages are transmitted in exactly the byte order written below.

All communication is done over USB serial at 230400 baud, 8N1 (8 data bits, no parity bits, 1 stop bit).

Messages From the Controller to the PC
--------------------------------------

|Message name|Data|Notes|
|------------|----|-----|
|Turn red LED on at address|0b10000011 <8 bit address>||
|Turn red LED off at address|0b10000010 <8 bit address>||
|Turn blue LED heartbeat on at address|0b10010001 <8 bit address>||
|Turn blue LED heartbeat off at address|0b10010000 <8 bit address>||
|Run bias calibration at address|0b10001000 <8 bit address>|Run the automated self-bias-calibration process on the specified node.|
|Set calibration values|0b10001100 <8 bit address> <unsigned 16 bit integer slope calibration> <signed 16 bit integer intercept calibration>|Set the slope and intercept calibration values for the specified node. The slope calibration value should be multiplied by 0.1. The intercept calibration value should be multiplied by 100.|
|Enable/disable calibration|0b10001110 <8 bit address> 00000cba|Bit `a` is whether slope cal is enabled, bit `b` is whether intercept cal is enabled, and bit `c` is whether bias cal is enabled. This setting does not persist across power cycles.|
|Get all known addresses|0b00000001|See "All known addresses" message for the response format.|

Messages From the PC to the Controller
--------------------------------------

|Message name|Data|Notes|
|------------|----|-----|
|All known addresses|0xFF 0b01010000 <number of addresses `N`> `N`\*<8 bit address>|The third byte is the number of addresses the controller knows about. After the third byte, every address is sent verbatim as a single unsigned 8-bit integer.|
|Sensor data at address|0xFF 0b0100<4-bit bitmask of whether the corresponding row's data is valid> <8 bit address> <8 bytes of row 4 data> <8 bytes of row 3 data> <8 bytes of row 2 data> <8 bytes of row 1 data>|The bitmask has bits of 0 when the data is known to be invalid. Each row's data is transmitted in four pairs of two bytes: 0bABC0xxxx xxxxxxxx 0b0000xxxx xxxxxxxx 0b0000xxxx xxxxxxxx 0b0000xxxx xxxxxxxx. Bit `A` is 1 if bias calibration was enabled for that measurement. Bit `B` is 1 if intercept calibration was enabled. Bit `C` is 1 if slope calibration was enabled. The `x` bits are the 12 bit data from the sensor. The sensor data for all known sensors is automatically sent every 100ms.|
