# GM E&amp;C bus cassette deck emulator

Original v0.1 version designed and tested for a 1997 Camaro CD headunit, and emulates a cassette deck for aux in.

Updated v2 version includes:

 - Bluetooth audio on ESP32 boards via I2S
 - Error correction
 - Better logging
 - Debug button function

Guide follows below (originally written at <https://www.reddit.com/r/Pontiac/comments/w3bfmb/guide_add_bluetooth_to_your_factory_19942003_gm/>):

# \[Guide\] Add Bluetooth to your factory 1994-2003 GM stereo 

If your stock GM Delco Theftlock stereo has a CD player and no cassette player, this guide will probably work for you.  This is tested on a 1997 Grand Am, but many cars from around 1994-2003, a.k.a. cars with the same type of radio, should work.  Check if your radio looks like this one: [https://www.ebay.com/itm/363552100489](https://www.ebay.com/itm/363552100489)  Some similar Delco stereos from other GM cars may also work (e.g. the Chevy ones seem to have the needed connector on the back), but no guarantees.

If your stereo has a cassette player, perhaps just buy one of these: [https://www.amazon.com/s?k=cassette+bluetooth+adapter](https://www.amazon.com/s?k=cassette+bluetooth+adapter)  The audio quality may be a bit lower, but the convinence of just buying a pre-made product is way higher.

If your radio doesn't have a CD player or a cassette player (so AM/FM only), I don't know if it will work, but if it doesn't, you could always pick up a CD player radio from eBay or a junkyard.

Note that only Bluetooth audio is supported, not hands-free calling.  It's possible that I may add this ability in the future, but the library I'm using for audio ([btAudio](https://github.com/tierneytim/btAudio)) doesn't currently support the needed protocol (HFP).

## Stuff you need

* [https://www.mouser.com/ProjectManager/ProjectDetail.aspx?AccessID=b6872231c2](https://www.mouser.com/ProjectManager/ProjectDetail.aspx?AccessID=b6872231c2)
* [https://www.amazon.com/Standard-Jumper-Solderless-Prototype-Breadboard/dp/B07H7V1X7Y](https://www.amazon.com/Standard-Jumper-Solderless-Prototype-Breadboard/dp/B07H7V1X7Y)
* A Micro USB cable.  If you don't have one laying around, I like these: [https://www.amazon.com/Micro-USB-Cable-Android-SMALLElectric/dp/B01N9P860N](https://www.amazon.com/Micro-USB-Cable-Android-SMALLElectric/dp/B01N9P860N)
* Optional: Soldering iron, solder, and decent flux (MG Chemicals 8341 is what I use).

## The steps

Start by following the Simple Audio guide here: [https://github.com/tierneytim/btAudio](https://github.com/tierneytim/btAudio)  (Note that your Mouser shipment should have both push headers and solder-on headers included for the DAC board, use whichever you prefer.)

Once that's tested and working with a test speaker, now you need the voltage conversion circuit.  Use the schematic here: [https://stuartschmitt.com/e\_and\_c\_bus/interface.html](https://stuartschmitt.com/e_and_c_bus/interface.html)  You can either build directly on the breadboard, or use the Perma-Proto board and some soldering to make the circuit, add headers, then plug the board into the breadboard.  By default, `EC_Rx` will connect to pin 19 on the ESP32 and `EC_Tx` to pin 22.

Now with that assembled, push this sketch to your ESP32: [https://github.com/qwertychouskie/GM-EandC-Cemu-v2/blob/master/Cemu%20v2%20Arduino%20ESP32/Arduino%20Code/EC\_Cassette/EC\_Cassette.ino](https://github.com/qwertychouskie/GM-EandC-Cemu-v2/blob/master/Cemu%20v2%20Arduino%20ESP32/Arduino%20Code/EC_Cassette/EC_Cassette.ino)

Make sure audio still works with the test speaker (default Bluetooth name is `GM Stereo BT`, but can be changed in the sketch).  Now onto hooking up to your car's stereo.  Follow a YouTube tutorial on how to pull out your stereo.  There will be an extra connector with no wire plugged in on the back of the radio, that is what we hook into.  The connector that hooks onto the radio, along with the female connectors that go in the connector, will be in the Mouser shipment.

Attach female connectors to the two thick wires of the UBEC, and 4 more wires (just snip off the ends of some jumpers and strip the wires).  Pick any color you want for the 4 wires as long as they don't overlap with each other or the black/red wires from the UBEC.  Once the female headers are on, insert them into the plastic connector, using this as your guide: [https://github.com/qwertychouskie/GM-EandC-Cemu-v2/blob/master/Documentation/9pin\_connector\_pinout.gif](https://github.com/qwertychouskie/GM-EandC-Cemu-v2/blob/master/Documentation/9pin_connector_pinout.gif)

The black wire of the UBEC goes to `Ground`, and the red wire to `Radio On Signal`.  The 4 remaining wires go to `Right Audio Signal`, `Left Audio Signal`, `Common Audio Signal`, and `Entertainment and Comfort Serial Data`.

Use extra jumper wires as extensions for the 6 wires, using electrical tape to keep the connections secure.  Make sure to keep the colors the same, so you know what goes where.  Plug the connectors into the radio, slide the radio into place, run the wires behind the dash to the little tray that sits underneath, and plug the wires into their appropriate locations on the breadboard.  Everything should work now: Turn the key on, connect your phone, and make sure the radio is on and showing the cassette icon.  Audio should now work.

\[EDIT\] Some images here: [https://imgur.com/a/OUy9BGF](https://imgur.com/a/OUy9BGF)
