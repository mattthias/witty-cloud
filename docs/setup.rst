Setup
=====

Serial connection
-----------------
The Witty Cloud board has two micro usb ports. Only the connector on the lower
board can be used to communicate with the board. The connector on the upper
board is only usable for power supply.


To connect to the board you need a micro usb cable and a terminal program. On
Linux install and use ''picocom''. You may need to add your user to the
 ''dialout'' to allow access to the serial ports::

	sudo adduser ${USER} dialout
	sudo apt-get install picocom
	picocom -b 115200 /dev/ttyUSB0

To end the session press ctrl + a and than ctrl + k


On MacOS use screen::

	screen /dev/tty....  115200
To quit the session press Ctrl + A and than k.

Upload and download code with mpfshell
--------------------------------------
Install mpfshell (https://github.com/wendlers/mpfshell) to up- and download
files into the flash filesystem of the board::

	 pip install --user mpfshell


Start the shell with::

	$HOME/.local/bin/mpfshell
	# or without the annoying colors
	$HOME/.local/bin/mpfshell --nocolor

On the command prompt type ''help'' to see all available commands. Connect to
your board, list the contents of the flash file system ::

	mpfs [/]> help

	Documented commands (type help <topic>):
	========================================
	EOF  cd     exec  get   lcd  lpwd  md    mput  mrm   put  repl
	cat  close  exit  help  lls  ls    mget  mpyc  open  pwd  rm

	mpfs [/]> open ttyUSB0
	Connected to esp8266
	mpfs [/]> ls

	Remote files in '/':

       		boot.py

	mpfs [/]>

Download a file::

	mpfs [/]> get boot.py

Upload a file::

	mpfs [/]> put main.py




WebREPL (web browser interactive prompt)
----------------------------------------

WebREPL (REPL over WebSockets, accessible via a web browser) is an
experimental feature available in ESP8266 port. Download web client
from https://github.com/micropython/webrepl (hosted version available
at http://micropython.org/webrepl), and configure it by executing::

    import webrepl_setup

and following on-screen instructions. After reboot, it will be available
for connection. If you disabled automatic start-up on boot, you may
run configured daemon on demand using::

    import webrepl
    webrepl.start()

The supported way to use WebREPL is by connecting to ESP8266 access point,
but the daemon is also started on STA interface if it is active, so if your
router is set up and works correctly, you may also use WebREPL while connected
to your normal Internet access point (use the ESP8266 AP connection method
if you face any issues).

Besides terminal/command prompt access, WebREPL also has provision for file
transfer (both upload and download). Web client has buttons for the
corresponding functions, or you can use command-line client ``webrepl_cli.py``
from the repository above.

See the MicroPython forum for other community-supported alternatives
to transfer files to ESP8266.
