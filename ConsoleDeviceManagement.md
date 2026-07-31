network device access via rj45/usb cabel+terminal emulator combo

install 

sudo apt install minicom | screen

## identify terminal device

dmesg | grep tty 

_[ 1234.567890] usb 1-1: pl2303 converter now attached to ttyUSB0_


**confirm it**

ls -l /dev/ttyUSB*
## using screen

screen /dev/ttyUSB0 9600
_ctrl+a then \ to quit_

## using minicom

sudo minicom -s

serial port setup:
- Serial device: _dev/ttyUSB0_
- Bps/Par/Bits: _9600 8N1_
- Flow control: _None_
- 