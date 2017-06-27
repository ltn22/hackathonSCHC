# hackathon for SCHC

## General Architecture

A LPWAN network is based on this architecture 


                                                 +------+
 ()    ()   ()         |                         |LPWAN-|
   ()  () () ()       / \         +---------+    | AAA  |
() () () () () ()    /   \========|    /\   |====|Server|  +-----------+
 ()  ()   ()        |             | <--|--> |    +------+  |APPLICATION|
()  ()  ()  ()     / \============|    v    |==============|    (App)  |
  ()  ()  ()      /   \           +---------+              +-----------+
 Dev         Radio Gateways           NGW

                       Figure 9: LPWAN Architecture
