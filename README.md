## About
This lib is created for scanning laser module GC-76000C produced by Micro photons(Shanghai)Technology Co., Ltd. The tech documentation of it provided by the corporation is my reference.

It can send control parameters (such as starting frequency) to the module, and request informations from it.

## Usage
### Function definitions
Here is the usage of all functions in the `LaserSerial` class, including their parameters and return values:

1. **`__init__`**:
   - **Parameters**:
     - `port`: String, specifies the serial port name, default is 'COM6'.
     - `baudrate`: Integer, specifies the baud rate, default is 9600.
     - `bytesize`: Specifies the byte size, default is `serial.EIGHTBITS`.
     - `parity`: Specifies the parity bit, default is `serial.PARITY_NONE`.
     - `stopbits`: Specifies the stop bits, default is `serial.STOPBITS_ONE`.
     - `xonxoff`: Boolean, specifies whether to enable software flow control, default is False.
     - `rtscts`: Boolean, specifies whether to enable hardware flow control, default is False.
     - `dsrdtr`: Boolean, specifies whether to enable DSR/DTR flow control, default is False.
     - `timeout`: Integer or float, specifies the timeout in seconds, default is 2 seconds.
   - **Return Value**: None.

2. **`calcBIP4`**:
   - **Parameters**:
     - `data`: Byte sequence, the data for which the BIP4 checksum needs to be calculated.
   - **Return Value**:
     - The BIP4 checksum.

3. **`convertCommand`**:
   - **Parameters**:
     - `data`: Byte sequence, the command data that needs to be converted.
   - **Return Value**:
     - The converted command, including the BIP4 checksum.

4. **`sendCommand`**:
   - **Parameters**:
     - `data`: Byte sequence, the command data that needs to be sent.
   - **Return Value**: None.

5. **`checkResponse`**:
   - **Parameters**: None.
   - **Return Value**:
     - Boolean, True if the response's BIP4 checksum is correct, otherwise False.

*Actually, the above functions are basically not directly called in use, the following functions are more convenient for direct use*

6. **`send_starting_frequency`**:
   - **Parameters**:
     - `starting_frequency`: Integer, specifies the starting frequency. Can choose between 196250 and 191150 (Their physical unit are GHz).
   - **Return Value**:
     - Boolean, indicates whether the command was executed successfully, (`True` means your are successful).

7. **`send_scan_step`**:
   - **Parameters**:
     - `scan_step`: Integer, specifies the scan step value. Can be set to 1 or 2 (Their physical unit are GHz).
   - **Return Value**:
     - Boolean, indicates whether the command was executed successfully.

8. **`send_stop_frequency`**:
   - **Parameters**:
     - `stop_frequency`: Integer, specifies the stop frequency. Can choose between 196250 and 191150 (Their physical unit are GHz).
   - **Return Value**:
     - Boolean, indicates whether the command was executed successfully.

9. **`send_scan_speed`**:
   - **Parameters**:
     - `scan_speed`: Integer, specifies the scan speed. Can be set to 1, 2, 3, or 4.
   - **Return Value**:
     - Boolean, indicates whether the command was executed successfully.

10. **`send_scan_control_command`**:
    - **Parameters**:
      - `command`: Byte, specifies the scan control command. Can be set to 0x01, 0x02, 0x00, for single scan, continuous scan, and stop scan respectively.(In fact, python is so "smart" that you can use Integer 1, 2 or 0 instead of 0x01, 0x02, 0x00.)
    - **Return Value**:
      - Boolean, indicates whether the command was executed successfully.

11. **`read_serial_number`**:
    - **Parameters**: None.
    - **Return Value**:
      - Integer, the device's serial number.

12. **`read_firmware_version`**:
    - **Parameters**: None.
    - **Return Value**:
      - Integer, the device's firmware version number.

### Example
You can initialize your laser module as follows:

```python
import serial
from laserlib import LaserSerial

laser = LaserSerial(port='COM6', baudrate=9600)
# Default setting is 
#     port='COM6', baudrate=9600, bytesize=serial.EIGHTBITS, parity=serial.PARITY_NONE, stopbits=serial.STOPBITS_ONE, xonxoff=False, rtscts=False, dsrdtr=False, timeout=2
```
Then you can use the functions like this:
```python
laser.send_starting_frequency(196250) #In fact, you can use the return value of this function, to ensure your command have been executed correctly. 
time.sleep(0.1) #It needs a delay time, but I dont know why...
laser.send_scan_step(1)
time.sleep(0.1)
laser.send_stop_frequency(191150)
time.sleep(0.1)
laser.send_scan_control_command(1)
time.sleep(0.1)
#You should set your laser module in above sequence.
laser.send_scan_speed(1)
time.sleep(0.1)
print(laser.read_serial_number())
time.sleep(0.1)
print(laser.read_firmware_version())
laser.closeSerial()
```

## License

This project is licensed under the MIT License - see the LICENSE file for details