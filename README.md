# EpsonESCPOS-PHP
This project is to allow you to print to your EPSON Thermal Printer via PHP. This has been tested on Windows XP, Windows 7, Centos and Debian as well as a Raspberry PI

#Installation and Configuration
Windows:
COM1

For linux devices it is best to do: dmesg | grep tty (This will show you your available serial devices)
LINUX:
/dev/ttyS1

RASPBERRY PI with PISERIAL
/dev/ttyAMA0

If you have issues with the font be garbage on the page then you will need to change Port Speed
Windows


    Open Start -> Computer -> Right Click -> Manage Computer -> Devices -> Ports -> Find your port and click Properties -> Click Configure -> Edit the Settings

    OR

    Open CMD -> Windows Key and R -> CMD -> Enter -> MODE [SERIALPORT]:[PORT SPEED],N,8,1,P
             For reference: http://www.computerhope.com/issues/ch000245.htm

LINUX
  You might want to try: stty -F /dev/tty[PORT] [SPEED]  however linux is usually out the box and it works.
  PLEASE NOTE: On Linux you may need to do: chmod a+x /dev/tty[DEVICE] as by default you only have Read Access to the Coms port
