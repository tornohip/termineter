# Getting Started #
## Background ##
Termineter interacts with smart meters using the ANSI C12.18 protocol over a serial connection.  The ANSI Type-2 optical probe that is in use must provide a serial interface.

## Basic Steps ##
Below is a summary of the basic steps to get started with Termineter after the environment has been configured.
  1. Connect the optical probe to the smart meter and start termineter
  1. Configure the connection options.  On Windows, this would be something like COM1 and on Linux something like /dev/ttyS0. Check Configuring the Connection for more details.
  1. Use the `connect` command, this will also check that the meter is responding.

## Configuring the Environment ##
Basic requirements:
  * [Python](http://www.python.org/) >= 2.6
  * [PySerial](http://pyserial.sourceforge.net/) >= 2.3.1
### Linux ###
**Running the install.sh script is optional.**  Termineter can be run using Python out of the source directory.

On Linux some USB optical probes use an FTDI chip to provide a serial connection and may not immediately show up in /dev/.  To remedy this use the `lsusb` command and search for a line saying "Future Technology Devices International" and next to it should be two hexadecimal strings separated by a colon. The first half is the Vendor ID and the second half is the Product ID as they need to be passed to the `modpobe` command. Proceed to use the `modprobe` command to load the ftdi-sio drivers with the appropriate vendor and product options.

Example:
<pre>
[someone@localhost ~]$ lsusb<br>
Bus 006 Device 002: ID 0403:f458 Future Technology Devices International, Ltd<br>
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub<br>
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub<br>
Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub<br>
Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub<br>
[someone@localhost ~]$ sudo modprobe ftdi-sio vendor=0x0403 product=0xf458<br>
[someone@localhost ~]$ ls /dev/tty* | grep USB<br>
/dev/ttyUSB0<br>
[someone@localhost ~]$<br>
</pre>
### Windows ###
Termineter can only be run from the source directory with Python, no installation process is provided.

## Configuring the Connection ##
This option is passed into the PySerial library so any valid value for the "port" parameter can be used.  If the PySerial library is new enough the serial\_for\_url function will be used which allows using RFC2217 serial bridge via a URL such as:
`rfc2217://192.168.90.128:2217`

Other options for the connection can be viewed and set using the Framework-level "Advanced Options".  These options include STOPBITS, BYTESIZE, and BAUDRATE.

## Other Notes ##
### The USEHEX Option ###
Some options in modules have a "USEHEX" option, this means that the corresponding value is provided as a string of hex characters and should be converted to binary before being sent to the meter.  This is also the case for the "PASSWORDHEX" option which when set to "True" means the password is expressed as a hex string.

### Resource Files ###
Like with Metasploit, a resource file can be executed automatically every time the framework starts. This can be helpful to avoid having to manually configure the connection options each time Termineter starts.  The user simply needs to place each command that they would like to run on a new line in the file.

On Linux the file can placed in ~/.termineter/console.rc and it will automatically be run every time Termineter starts.  Additionally a resource file can be explicitly loaded on start by using the -r/--rc-file options.