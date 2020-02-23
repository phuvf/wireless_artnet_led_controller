Wireless ArtNet LED controller: Documentation
=============================================

The wireless ArtNet LED controller will control up to 170 pixels of individually addressable LEDs using the widely supported ArtNet protocol over standard WiFi networks.

It drives a variety of individually addressable LED products (both strips and matrices) and connects to standard WiFi networks over which it receives ArtNet data.

### Product visuals
<img src="https://github.com/phuvf/wireless_artnet_led_controller/blob/master/img/box.jpg" width="500">
<img src="https://github.com/phuvf/wireless_artnet_led_controller/blob/master/img/plan.png" width="500">

### Physical

The unit comprises an outer casing measuring 75x50x30mm. Electrical connections are a microUSB socket at one end, and a four-pole terminal socket at the other. Matching connectors for this are widely available - such as [this listing from RS](https://uk.rs-online.com/web/p/pcb-terminal-blocks/8971216/) in the UK.

### Power

For low-power strips, the unit can be powered via the Micro USB connector. This can supply c. 1.5A @ 5V to your LEDs via the green connector.

For higher current applications, power the controller by supplying 5V into the green connector. The controller draws around 200mA when at full load.

Internally, the controller has a 1000μF capacitor between +5V and GND to buffers sudden changes in the current drawn by the LEDs.

### Signal

The controller can drive two families of LEDS:

*   WS2812B/SK6812/NeoPixel, using just the DATA line
*   APA102/SK98225/DotStar, using both the DATA and CLK lines

If the CLK line is not required, just leave it disconnected.

For each family, a range of colour orders are available:

Type

Notes

NeoPixel RGB

NeoPixel RBG

NeoPixel GRB

Most common option

NeoPixel BRG

NeoPixel RGBW

NeoPixel GRBW

DotStar RGB

DotStar RBG

DotStar GRB

DotStar GBR

DotStar BGR

Most common option

DotStar LRGB

L = global dimmer channel, Luminance

DotStar LRBG

DotStar LGRB

DotStar LGBR

DotStar LBRG

DotStar LBGR

### ArtNet support

The unit will drive one ArtNet universe of data (512 channels). That's 170 pixels for three-channel LEDs, or 128 pixels for four-channel LEDs.

Universes available range from 0 to 127 (technically 8 subnets of 16 universes each).

The unit supports ArtNet polling, so if your ArtNet sender (e.g. lighting desk) supports device discovery, the controller will appear on the list of devices available on the network, with the ID 'ArtNet LED controller xxxxx'.

ArtNet packets can be unicast or broadcast to this device. If your sender supports polling correctly, it'll automatically unicast the correct subnet of data to the controller.

During operation, the acheivable frame rate will depend on the general WiFi environment. With good WiFi, the device can sustain 40fps without issue.

A single universe at 40fps consumes 0.2MBps.

### Unit setup

<img src="https://github.com/phuvf/wireless_artnet_led_controller/blob/master/img/interface-ipad.png" width="500">

Unit setup is performed using an on-board web interface. To start, press the recessed reset button on the side.

Note: You don't need to hold this button down - the unit will reboot into setup mode immediately. When the unit is connecting to WiFi, the reset button does not work - see below.

When in setup mode, the device will act as a WiFi access point, and create a wireless network named 'ArtNet LED controller xxxxx', where xxxxx the unit ID.

Using a computer or phone, connect to this wireless network (there is no password) and browse to 192.168.0.1, where you'll be presented with a configuration screen.

Here you can:

*   Set the SSID and passsword of your ArtNet network
*   Set the IP, subnet and gateway addresses for the device (or choose to use DHCP)
*   Set the ArtNet universe of the device
*   Select the correct LED type

Once complete, hit 'Save and reboot'. The unit will re-configure itself and attempt to connect to the wireless network specified.

Note: the unit does not need to be in setup mode to connect to the config page. Just make sure you're on the same network as the unit, and substitute the 192.168.0.1 address for the IP address of the configured unit.

Warning: There's no password on the web interface, so your unit could be re-configured by anyone on the same network.

### Test pattern

On boot, the unit will display a test pattern on connected LEDs in the order red-green-blue. It'll then hold on blue until it has estalished a wireless connection.

This helps in determining the correct colour order for the LEDs connected.

### WiFi reconnection

On boot, the unit will attempt to connect to the WiFi network using saved details. If this fails (for example, if the router is off) it'll retry every 30s until a connection is established.

Similarly, if an established connection is lost during operation, the unit will try to reconnect every 30s.

### On-board LED codes

The on-board LED give status information for the device:

Flash pattern

Description

Continuous on

Unit is booting, or trying to connnect to WiFi. Reset button will not work.

Long on, short off

Unit is in setup mode.

Short flash every 3s

Unit is in normal mode and has connected to WiFi, but is not receiving ArtNet data.

Rapid irregular flickering

Unit receiving and processing ArtNet packets. Each flash is a packet being processed.

Regular rapid flash, 10/s

Unit has failed to connect to Wifi.

### Credits

This unit uses the excellent [NeoPixelBus](https://github.com/Makuna/NeoPixelBus) library by Makuna.