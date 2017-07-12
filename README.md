# IETF99 hackathon on SCHC

## General Architecture

A LPWAN network is based on this architecture [https://tools.ietf.org/html/draft-ietf-lpwan-overview]

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

For this hackathon, two technologies has been selected: LoRaWAN and Sigfox. They
both operate in the 868 Mhz band (in Europe). 

For the Sigfox network, radio and network gateways will be based on the live service 
provider infrastructure. For the hackathon, Sigfox will provide access, relaxing 
restictions on the number of uplink and downlink messages sent per day.  Devices 
will have to be register during the hackathon on a special account. Device ID and 
**PAC number** are needed to proceed to the registration.


A LoRaWAN network will be provided inside the room. The NGW will be based on an open source
implementation of LoRaWAN 1.0.4 [https://github.com/brocaar/loraserver]. 
Devices will have to register on that NGW either in OTAA or in ABP. 

## Communicating with devices

Both network will send a POST message when data is received by their NGW. The
URI will be specified at the device registration. The application server can be either located
on a machine on the hackathon network or outside on the cloud. The machine must have
a stable IP address or be registered on the DNS.

### Sigfox POST message format

The POST message sent by the gateway will have the following format:

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

The POST message sent by the gateway will have the following format: 

~~~~
RAW JSON { 
  "devEUI": "70b3d54997ceca03",
  "appEUI": "0000000000000000",
  "fPort": 5,
  "gatewayCount": 4,
  "rssi": 78,
  "data": "AcaZBQkNEPT1GV0aDACEGRigghkLNBoAAYc+OgAArF9FaGVsbG8=",
  "loRaSNR": 6,
  "frequency": 86810000000,
  "dataRate": { "modulation": "LORA", "spreadFactor": 7, "bandwidth": 125 } 
}
~~~~

Where data sent by the device are base64 encoded.

A response can be POST'ed to the NGW, destined to the device. This message must 
have the following format:

~~~~
 {
			"fport" : 2,
			"data" : data,
			"devEUI" : ES_DID
}
~~~~

where data are base64 encoded.

# SCHC architecture

The SCHC compression mechanism is described in [https://datatracker.ietf.org/doc/draft-ietf-lpwan-ipv6-static-context-hc/?include_text=1]

The SCHC compressor/decompressor will be located in the device and in the application server. 

## SCHC rules 

The rule format is not specified in the draft. For interoperability between implementations, a JSON format can be specified.

The first test is based on IPv6/UDP compression-decompression. 
~~~~
rule_coap1 = {"ruleid"  : 1,
             "content" : [["IPv6.version",      1,  "bi", 6,                  "equal",  "not-sent"],
                          ["IPv6.trafficClass", 1,  "bi", 0x00,               "equal",  "not-sent"],
                          ["IPv6.flowLabel",    1,  "bi", 0x000000,            "equal",  "not-sent"],
                          ["IPv6.payloadLength",1,  "bi", None,               "ignore", "compute-length"],
                          ["IPv6.nextHeader",   1,  "bi", 17,                 "equal",  "not-sent"],
                          ["IPv6.hopLimit",     1,  "bi", 30,                 "ignore", "not-sent"],
                          ["IPv6.prefixES",     1,  "bi", 0xFE80000000000000, "equal", "not-sent"],
                          ["IPv6.iidES",        1,  "bi", 0x0000000000000001, "equal", "not-sent"],
                          ["IPv6.prefixLA",     1,  "bi", [0x2001066073010001,
                                                           0x2001123456789012,
                                                           0x2001123456789013,
                                                           0xFE80000000000000],"match-mapping", "mapping-sent"],
                          ["IPv6.iidLA",        1,  "bi", 0x0000000000000002, "equal", "not-sent"],
                          ["UDP.PortES",        1,  "bi", 5680,               "MSB(13)", "LSB"],
                          ["UDP.PortLA",        1,  "bi", 5680,               "MSB(13)", "LSB"],
                          ["UDP.length",        1,  "bi", None,               "ignore", "compute-length"],
                          ["UDP.checksum",      1,  "bi", None,               "ignore", "compute-checksum"],
                       ]}
~~~~
This rule will result in sending over the air:

 * 1 byte rule ID
 
 * 2 bits for the IPv6 PrefixLA 
 
 * 3 bits for the UDP PortES
 
 * 3 bits for the UDP PortLA

 * the UDP payload

 



