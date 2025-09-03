# Installation
Inside the openocd folder, run
```
sudo apt install make libtool-bin pkg-config libusb-1.0.0-dev
./bootstrap
./configure --enable-stlink
make
sudo make install
```

# Usage
## OTP Write
openocd -f interface/stlink.cfg -f target/bluenrg-x.cfg -c "hla_serial <stlink-sn>" -c "init" -c "halt" -c "bluenrg-x otp 0 <otp-address> <value>" -c "reset halt" -c "exit"
### Example
openocd -f interface/stlink.cfg -f target/bluenrg-x.cfg -c "hla_serial 34001600050000304131574E" -c "init" -c "halt" -c "bluenrg-x otp 0 0x10001b00 0x1234" -c "reset halt" -c "exit"

#### Output
```
leo@dev:~/openocd$ openocd -f interface/stlink-v2.cfg -f target/bluenrg-x.cfg -c "hla_serial 34001600050000304131574E" -c "init" -c "halt" -c "bluenrg-x otp 0 0x10001b00 0x1234" -c "reset halt" -c "exit"
Open On-Chip Debugger 0.12.0+dev-g9a2c12a49 (2025-09-02-22:52)
Licensed under GNU GPL v2
For bug reports, read
	http://openocd.org/doc/doxygen/bugs.html
WARNING: interface/stlink-v2.cfg is deprecated, please switch to interface/stlink.cfg
Info : auto-selecting first available session transport "hla_swd". To override use 'transport select <transport>'.
Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
DEPRECATED! use 'adapter serial' not 'hla_serial'
Info : clock speed 4000 kHz
Info : STLINK V2J37S7 (API v2) VID:PID 0483:3748
Info : Target voltage: 3.305023
Info : [bluenrg-1.cpu] Cortex-M0+ r0p1 processor detected
Info : [bluenrg-1.cpu] target has 4 breakpoints, 2 watchpoints
Info : [bluenrg-1.cpu] Examination succeed
Info : starting gdb server for bluenrg-1.cpu on 3333
Info : Listening on port 3333 for gdb connections
Warn : target was in unknown state when halt was requested
[bluenrg-1.cpu] halted due to debug-request, current mode: Thread 
xPSR: 0x21000000 pc: 0x10044c98 msp: 0x20007f30
bluenrgx otp ok
Error executing event halted on target bluenrg-1.cpu:
/usr/local/bin/../share/openocd/scripts/target/bluenrg-x.cfg:59: Error: can't read "JTAG_IDCODE_B2": no such variable
in procedure 'ocd_process_reset' 
in procedure 'ocd_process_reset_inner' called at file "embedded:startup.tcl", line 1353
at file "/usr/local/bin/../share/openocd/scripts/target/bluenrg-x.cfg", line 59
[bluenrg-1.cpu] halted due to debug-request, current mode: Thread 
xPSR: 0xf1000000 pc: 0x1000177c msp: 0x20010000
Warn : Flash driver of bluenrg-1.flash does not support free_driver_priv()
```

## OTP Read
openocd -f interface/stlink-v2.cfg -f target/bluenrg-x.cfg -c "hla_serial <stlink-sn>" -c "init" -c "$target_name mdw <otp-address> <size>" -c "exit"
### Example
openocd -f interface/stlink-v2.cfg -f target/bluenrg-x.cfg -c "hla_serial 34001600050000304131574E" -c "init" -c "$target_name mdw 0x10001800 200" -c "exit"

#### Output
```
leo@dev:~$ openocd -f interface/stlink-v2.cfg -f target/bluenrg-x.cfg -c "hla_serial 34001600050000304131574E" -c "init" -c "$target_name mdw 0x10001800 200" -c "exit"
Open On-Chip Debugger 0.12.0
Licensed under GNU GPL v2
For bug reports, read
	http://openocd.org/doc/doxygen/bugs.html
WARNING: interface/stlink-v2.cfg is deprecated, please switch to interface/stlink.cfg
Info : auto-selecting first available session transport "hla_swd". To override use 'transport select <transport>'.
Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
DEPRECATED! use 'adapter serial' not 'hla_serial'
Info : clock speed 4000 kHz
Info : STLINK V2J37S7 (API v2) VID:PID 0483:3748
Info : Target voltage: 3.311453
Info : [bluenrg-1.cpu] Cortex-M0+ r0p1 processor detected
Info : [bluenrg-1.cpu] target has 4 breakpoints, 2 watchpoints
Info : starting gdb server for bluenrg-1.cpu on 3333
Info : Listening on port 3333 for gdb connections
[bluenrg-1.cpu] halted due to breakpoint, current mode: Thread 
xPSR: 0xf1000000 pc: 0x1000177c msp: 0x20010000
0x10001800: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001820: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001840: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001860: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001880: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x100018a0: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x100018c0: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x100018e0: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001900: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001920: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001940: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001960: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001980: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x100019a0: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x100019c0: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x100019e0: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001a00: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001a20: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001a40: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001a60: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001a80: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001aa0: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001ac0: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001ae0: ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 
0x10001b00: 00001234 ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff
```

## OTP Write + Read

### Example
```
leo@dev:~/openocd$ openocd -f interface/stlink.cfg -f target/bluenrg-x.cfg -c "hla_serial 34001600050000304131574E" -c "init" -c "halt" -c "bluenrg-x otp 0 0x10001b04 0x12345678" -c "reset halt" -c "exit"

leo@dev:~$ openocd -f interface/stlink-v2.cfg -f target/bluenrg-x.cfg -c "hla_serial 34001600050000304131574E" -c "init" -c "$target_name mdw 0x10001b04 1" -c "exit"
.
.
.
0x10001b04: 12345678 
```
