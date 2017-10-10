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
