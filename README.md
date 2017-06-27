# hackathon for SCHC

## General Architecture

A LPWAN network is based on this architecture [draft-ietf-lpwan-overview-04]

~~~~
                                                 +------+
 ()    ()   ()         |                         |LPWAN-|
   ()  () () ()       / \         +---------+    | AAA  |
() () () () () ()    /   \========|    /\   |====|Server|  +-----------+
 ()  ()   ()        |             | <--|--> |    +------+  |APPLICATION|
()  ()  ()  ()     / \============|    v    |==============|    (App)  |
  ()  ()  ()      /   \           +---------+              +-----------+
 Dev         Radio Gateways           NGW

                       Figure 9: LPWAN Architecture
~~~~

For the hackathon two technologies has been selected: LoRaWAN and Sigfox. They
both operate in the 868 Mhz band. 

For Sigfox network, radio gateway and network gateway will be based on the provider
infrastructure. For the hackathon, Sigfox will provide an access without restictions 
on the number of uplink and downlink messages sent per day. For this devices 
will have to be register during the hackathon on a special account. Device ID and 
**PAC number** are needed to proceed to the registration. 

LoRaWAN network will be provided inside the room on a NGW based on an open source
implementation of the LoRaWAN 1.0.4 [https://github.com/brocaar/loraserver]. 
Devices will have to be register on that NGW either in OTAA or in ABP. 

## Communicating with devices

Both network will send a POST message when data will be received by the NGW. The
URI will be specified at the device registration. The server can be either located
in a machine on the hackathon network or outside on the cloud. The machine must have
a stable IP address or be registred on the DNS.

### Sigfox POST message format

The POST message send by the gateway will have the following format:

~~~~
{
 "time": {time},
 "device": "{device}",
 "data": "{data}",
 "station": "{station}",
 "ack": {ack}
}
~~~~

*data will be coded in ASCII using the hexadecimal format* **To be verified **

The POST response can contain a message to the device. This message must be 
exactly 8 byte long.

### LoRaWAN POST message format

The POST message send by the gateway will have the following format: 

~~~~
RAW JSON { 
  devEUI: '70b3d54997ceca03',
  appEUI: '0000000000000000',
  fPort: 5,
  gatewayCount: 4,
  rssi: 78,
  data: 'AcaZBQkNEPT1GV0aDACEGRigghkLNBoAAYc+OgAArF9FaGVsbG8=',
  loRaSNR: 6,
  frequency: 86810000000,
  dataRate: { modulation: 'LORA', spreadFactor: 7, bandwidth: 125 } 
}
~~~~

Where data sent by the device are base64 encoded.

The POST response can contain a message to the device. This message must 
respect the following format:

~~~~
 {
			'fport' : 2,
			'data' : data,
			'devEUI' : ES_DID
}
~~~~

where data are base64 encoded.


