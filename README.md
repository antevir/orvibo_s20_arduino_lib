# OrviboS20_Arduino
This is an unofficial Arduino library for Orvibo WiWo S20 smart plugs using a ESP8266. It supports:
* Subscription (keeps track of the current relay state)
* Setting relay state
* WiFi "pairing" (i.e. configure a S20 plug with a new WiFi SSID and passkey)
* Multiple devices

Credits to the protocol information available here: https://github.com/Grayda/node-orvibo

## Rationale
The S20 devices are not sold anymore so why bother making a library? I happen to have some WiWo S20 lying around and from time to time I have projects that requires controlling 230V devices. Instead of using DIY relay boards that is sometimes not that safe it seems like a better idea to control the 230V device with a WiWo S20 connected to an ESP8266.

## Documentation
The library is separated in two parts that are handled separately; controlling S20 devices and WiFi "pairing" of S20 devices.

### OrviboS20Device
This class represents one S20 device. I.e. if you want to control several devices you need to create several instances of this class like:
```cpp
OrviboS20Device s20_1;
OrviboS20Device s20_2;
```
When you want to control a specific device you can specify the MAC in the constructor like:
```cpp
uint8_t mac1[] = { 0xac,0xcf,0x23,0x35,0x55,0xb6 };
OrviboS20Device s20_1(mac1);
```
You can also set an user defined name that can be useful when sharing callbacks for multiple devices:
```cpp
OrviboS20Device s20_1("Plug1");
OrviboS20Device s20_2("Plug2");
```

#### Callbacks
There are a couple of callbacks that you can set for each devices that works as a notification when something happens. These are:
```
onConnect
onDisconnect
onStateChange
```
Please see the [examples](https://github.com/antevir/OrviboS20_Arduino/tree/master/examples) how these works.

#### bool isConnected()
Returns `true` if the S20 is connected. The device is regarded as connected if it is found using the Orvibo UDP protocol.
```cpp
bool state = s20.isConnected();
if (state) {
  Serial.println("S20 device is connected");
}
```

#### setState(state)
Sets the relay of the S20 device to the specified state (`true` = ON):
```cpp
if (something) {
  // Turn on relay
  s20.setState(true);
}
```

#### getState()
Gets the last known state of the S20 relay (`true` = ON):
```cpp
bool state = s20.getState();
if (state) {
  Serial.println("Relay is on");
}
```
#### getMac()
Returns MAC address of the device. If no MAC was specified in the constructor it will return 00:00:00:00:00:00 until a new Orvibo device connects.
```cpp
const uint8_t *mac = s20.getMac();
Serial.printf("MAC: %02x:%02x:%02x:%02x:%02x:%02x\n", mac[0], mac[1], mac[2], mac[3], mac[4], mac[5]);
```

#### getName()
Returns the user name set when the device was created.
```cpp
OrviboS20Device s20("MyPlug");
Serial.print("The name of the device is: ");
Serial.println(s20.getName());
```
Please see the [ToggleMultiplePlugs example](https://github.com/antevir/OrviboS20_Arduino/blob/master/examples/ToggleMultiplePlugs/ToggleMultiplePlugs.ino) when this can be useful.

## Example code
There are several examples available [here](https://github.com/antevir/OrviboS20_Arduino/tree/master/examples). When you install this arduino library you will also find the examples in `File` -> `Examples` ->`OrviboS20` 
