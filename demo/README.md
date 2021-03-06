# Auto Eyes Demo
__LED Light Strip Controlled by Python on Raspberry PI__

Project _Auto Eyes_ is about creating common communication protocols for common road scenarios, however,
technology works better when demonstrated in the real world.

Follow this guide to build your own real world example and see _Auto Eyes_ work with your own eyes.  

# What it Does

Point a mobile phone at an _Actor_ and the lights on the LED strip follow the _Actor_ giving the sense the _Actor_ is being watched.

<img src="https://docs.google.com/drawings/d/e/2PACX-1vQ5ohc6EMHRBjyLlLMU2Uz-lvJSsrhqxNgJnNhsBcoGKWemuTr97Ohl6objAnpldiR5kEUUdCSqlzxz/pub?w=960&amp;h=720">


`Video demo coming soon`

## Purchase

For approximately $150, you can set up this prototype, although the costs could 
be reduced by a knowledgeable person since this parts list caters to ease of setup. 

* 1 - $37 [5 meter Medium Resolution LED Strip](https://smile.amazon.com/gp/product/B00VQ0D2TY/ref=oh_aui_detailpage_o00_s01?ie=UTF8&psc=1)
* 1 - $26 [Power Supply](https://smile.amazon.com/gp/product/B01LXN7MN3/ref=oh_aui_detailpage_o00_s01?ie=UTF8&psc=1)
* 1 - $80 [Raspberry PI Kit](https://smile.amazon.com/gp/product/B07BCC8PK7/ref=oh_aui_detailpage_o00_s00?ie=UTF8&psc=1)
* 1 - $5  [Jumper Wires](https://smile.amazon.com/gp/product/B077N58HFK/ref=oh_aui_detailpage_o00_s00?ie=UTF8&psc=1)

__Optional:__
* 1 - $23 [1 meter High Resolution LED Strip](https://smile.amazon.com/gp/product/B01DLYSH6U/ref=oh_aui_detailpage_o03_s01?ie=UTF8&psc=1) in addition to or instead of the 5 meter light string

## Assemble

1. Setup the Raspberry PI with the included USB power supply, per the instructions that came with the kit.  
   1. Install the _Raspian Lite_ OS without the desktop environment.
1. Shut the Raspberry PI  down once you confirm it works (either by direct keyboard+monitor or ssh).
1. Connect the LED Strip power wires to the _female DC connector_ using a small screwdriver.
   1. Negative (-) connects to black.
   1. Positive (+) connects to red.  
1. Connect the LED 3pin JST-SM connector to the Raspberry PI GPIO connector with the jumper wires
![Connection Photo](images/hardware-connection-photo.jpg)
   1. Use the 3 wire with a female JST-SM connector provided with the LED strip
   1. Choose a red, green and blue jumper wire from the jumper wires  
   1. Cut and strip one end of each of the jumper wires
   1. Connect each jumper wire to the corresponding color of the 3 wire JST-SM connector
       1. Ideally solder each wire, but twist + tape will suffice.
   1. Connect each wire to the GPIO board
       1. Red (Power) -> Pin 2
       2. Black (Ground) -> Pin 6
       3. Green (Data) -> Pin 12
![GPIO Diagram](images/canakit-gpio-diagram.png)
Source: [CanaKit](https://www.canakit.com/Media/CanaKit-Raspberry-Pi-Quick-Start-Guide-3.2.pdf)
1. Plug the _female DC connector_ to the _male DC connector_ and plug power cord into the wall.
   1. Notice this powers both the LED strip and the Raspberry PI so do not use the USB power supply.
1. Confirm the Raspberry PI is powered up and available. 
1. Continue with the software setup to confirm the light strip is working.

## Software Setup 


### Test the Light Strip

Demonstrate the light strip works by running a quick test in python3.

`python3 --version` -> `Python 3.5.3` (known to work)

Install PIP used to install python packages:

`sudo apt-get install python3-pip`

Install the light strip python3 driver:

`sudo pip3 install rpi_ws281x`

Download the test python

`wget https://raw.githubusercontent.com/rpi-ws281x/rpi-ws281x-python/master/examples/strandtest.py`

Run the test:

`sudo python3 strandtest.py -c`

__Expected Results:__ You should see the first 16 LEDs cycle through Red, Green, Blue and some rainbow cycles.

Modify the test to illuminate all of your pixels. 

`nano strandtest.py`

Change the LED Count on line 13:

`LED_COUNT      = 16` -> `LED_COUNT      = 144`

* 1 Meter Strip = 144 (High Resolution 144/m)
* 5 Meter Strip = 300 (Medium Resolution 60/m)

Run the test again and you should see your entire strip light up.

### Auto Eyes Setup

Change the hostname to `autoeyes`.

`sudo raspi-config` -> `2` -> `N1` -> `Ok` -> `autoeyes` -> `Ok`  then Finish and reboot

Confirm hostname changed and determine IP address.

`ping autoeyes.local` 

Install git so you can download the source.

`sudo apt-get install git`

Clone the AutoEyes source code.

`git clone https://github.com/aroller/autoeyes`

Install package dependencies.

`sudo python3 setup.py install`

Run the API connecting to the LED strip.

`cd ~/autoeyes/demo/python`

`sudo AV_EYES_LED_MODE=true python3 api.py`

Confirm LED Strip shows splash sequence of two white lights looping around in opposite directions ending with 4 segments of different colors.

`curl -X PUT "http://autoeyes.local:9090/v1.0/actors/abc123?bearing=5" -H "accept: application/json"`

Confirm the first 5 LEDs are lit in white.

#### Get to Know the API 

Visit the API in a web browser of a computer on the same network.

`http://autoeyes.local:9090/v1.0/ui/`

Use the web api user interface to PUT various combinations of bearing, action, direction, urgency. You should see the LED lights change accordingly.

#### Run the Compass Client

View the compass UI in a web browser on a computer.

`http://autoeyes.local:9090/client/compass.html`


In a mobile web browser you may need to access by ip address. 

`http://{autoeyes.local ip address}/client/compass.html` 



## Demo 

__Auto Eyes__ scenario demos _To Be Determined_.

