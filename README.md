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
will have to be register during the hackathon on a special account. Device ID and P
AC number are needed to proceed to the registration. 

LoRaWAN network will be provided 
