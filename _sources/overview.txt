Overview
========

Pinout
------

.. image:: img/witty_cloud_pinout.jpg
    :alt: Witty Cloud ESP8266 board
    :width: 640px


The AI Thinker Witty Cloud board (Image stolen from http://www.schatenseite.de/en/2016/04/22/esp8266-witty-cloud-module/).


The witty board consists of two stacked pcbs (printed circuit board). The upper
pcb carries pin headers to connect both pcbs, the ESP8266 microcontroller (the
big metal square), a multi color LED (Light Emitting Diode), a small blue LED,
a light dependent resistor (LDR), a push button and a micro usb connector
(only for power).

The lower board carries an usb-to-serial converter chip (CH341), a micro usb
port (for programming and for power), two push buttons (one is the reset button)
and the pin headers to connect to the upper pcb.

The boards come pre-flashed with a firmware bound to a chinese cloud service.
As the ESP8266 is a very cheap and powerful SoC alternative firmwares where
developed by communities around the world. The is a LUA interpreter
(https://nodemcu.readthedocs.io/en/master/), support for the Arduino IDE
(https://arduino-esp8266.readthedocs.io/en/latest/), a Javascript Interpreter
(https://www.espruino.com/EspruinoESP8266) and the most important:  MicroPython
(http://micropython.org/).

To flash the MicroPython firmware on the board follow these instructions:

http://docs.micropython.org/en/v1.9.2/esp8266/esp8266/tutorial/intro.html#intro


"Hello World"
-------------

Connect the micro usb connector in the lower board with your computer and open
the terminal application::

	picocom -b 115200 /dev/ttyUSB0

You will see the MicroPython REPL (Read Evaluate Print Loop) prompt::

	>>>
Now start to type Python commands!

::

	>>> print("Hello MicroPython!")
	Hello MicroPython!
	>>>

Micropython is a Python 3 implementation compact enough to run in just 256k of
code space and 16k of RAM. It aims to be as compatible with normal Python as
possible.

::

	>>> 123456789 * 123456789
        15241578750190521
	>>> a = 1
	>>> while a < 10:
	...     print("%d " % a, end='')
	...     a+=1
	...
	...
	...
	1 2 3 4 5 6 7 8 9 >>>
	>>>

"Hello World" microcontroller style
------------------------------------

The microcontroller "Hello World" is to blink a LED.

::

	>>> import machine
	>>> green = machine.Pin(12, machine.Pin.OUT)
	>>> green.on()
	>>> green.off()
	>>> import time
	>>> while True:
	...     time.sleep(1)
	...     green.on()
	...     time.sleep(1)
	...     green.off()
	...
	...
	...
More explanation here: http://docs.micropython.org/en/v1.9.2/esp8266/esp8266/tutorial/pins.html

Using the integrated hardware
-----------------------------

Multi-color LED
~~~~~~~~~~~~~~~

The multi-color LED consists of three single LEDs. They can be controlled
separately.

====== =================
color  connected to pin
====== =================
red    15
green  12
blue   13
====== =================


Use the ''machine'' module to create ''Pin'' objects.

::

	>>> import machine
	>>> red = machine.Pin(15, machine.Pin.OUT)
	>>> green = machine.Pin(12, machine.Pin.OUT)
	>>> blue = machine.Pin(13, machine.Pin.OUT)

These pins are digital only 1 (LED turned on) or 0 (LED turned off) is possible.

::

	>>> red.on()
	>>> green.on()
	>>> blue.on()

There is no default function to toggle the state of a pin but this is simple
to implement::

	def toggle(led):
		led.value(not led.value())


To fade the LEDs use Pulse Wide Modulation (http://docs.micropython.org/en/v1.9.2/esp8266/esp8266/tutorial/pwm.html):

::

	>>> import machine
	>>> blue = machine.PWM(machine.Pin(13, machine.Pin.OUT))
	>>> blue.init()
	>>> blue.freq(1000)
	>>> for i in range(0, 314):
	...     blue.duty(int(math.sin(i/100)*1000))
	...     time.sleep_ms(10)


PWM also allows to mix the three different colors (as you TV worked back than).

Light Dependent Resistor (LDR)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Light Dependent Resistor (LDR, https://en.wikipedia.org/wiki/Photoresistor)
is connected to the ADC (Analog to Digital Conversion) pin of the ESP8266.

The read() function returns values between 0 (0.0V) and 1024 (1.0V). For the
LDR the values are 0 (no light) and 1024 (bright light.)

::

	>>> import machine
	>>> ldr = machine.ADC(0)
	>>> ldr.read()
	623
	>>>

Push buttons
~~~~~~~~~~~~

The buttons are connected to the pins 4 (push button) and 0 ("flash" button).
The third button is connected to the RST pin and resets the board when pushed.

::

  	>>> import machine
	>>> button = machine.Pin(4, machine.Pin.IN)
	>>> flash_button = machine.Pin(0, machine.Pin.IN, machine.Pin.PULL_UP)
	>>> button.value()
	1
	>>> button.value()  # pressed
	0
	>>> flash_button.value() # pressed
	0
