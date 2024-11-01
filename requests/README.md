# Requests
This part is based on Brawl Stars, but I'll probably do the same for Clash of Clans to compare.  
Anyway, the methods for reading / modifying payloads and responses are the same across all Supercell games, so while this is for Brawl Stars, it could apply to any Supercell game.  
## Parts
- [Requests from client](#requests-from-client)
  - [Crypted payload analyse](#crypted-payload-analyse)
  - [Decrypting payloads](#decrypting-payloads)
  - [Decrypted payloads analyse](#decrypted-payloads-analyse)
  - [Payloads modifications](#payloads-modifications)
- [Responses from server](#responses-from-server)
  - [Different types of responses](#different-types-of-responses)
  - [Crypted response analyse](#crypted-response-analyse)
  - [Decrypting internal responses](#decrypting-internal-responses)
  - [Decrypted internal responses analyse](#decrypted-internal-responses-analyse)

## Requests from client

### Crypted payload analyse  
crypted request example : [crypted-001.txt](https://github.com/slayy2357/mimi/blob/main/requests/dump/crypted-001.txt)  
If you modify encrypted payloads you can easily get this (or the same but with sync error):
![alt text](https://raw.githubusercontent.com/slayy2357/mimi/refs/heads/main/requests/pictures/1.png)

### Decrypting payloads  
Analysing modules
```text
libCommonCrypto.dylib
CCSHA256 : no
CCSHA1   : no
CCMD5    : no
CCCrypt  : 1 call
```
CCCrypt call : [cccrypt-001.txt](https://github.com/slayy2357/mimi/blob/main/requests/dump/cccrypt-001.txt)
``` text
libboringssl.dylib
SSL_write          : 20+ call
SSL_read           : 20+ call
```
SSL_read call : [ssl_read-001.txt](https://github.com/slayy2357/mimi/blob/main/requests/dump/ssl_read-001.txt)  

So, we can read some payloads before encryption through SSL_write, SSL_read, and CCCrypt, but those aren't the main payloads. What we want to intercept is the payload sent through the send function before encryption, as that's how the game communicates with the server—for example, when joining a game, using emotes, etc  

Alright, I haven't found the encryption function yet, but now I'm able to read and modify every payload before encryption.

### Decrypted payloads analyse  
(!) : Decrypted payloads are not necessarily readable. Decrypted means that the structure is logical and will be the same for all payloads  

Dump of "understandable" payload found for the moment :  

[the first request](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-001.txt)  
``` text
Analyse :
00000090  00 00 00 28 36 31 62 32 61 64 38 36 32 64 63 35  ...(61b2ad862dc5
000000a0  61 65 62 62 30 65 31 30 61 30 30 33 32 39 62 62  aebb0e10a00329bb
000000b0  66 66 32 65 66 37 34 38 61 39 62 64 00 00 00 09  ff2ef748a9bd....
//this look like an ID, if you modify it you can't access to the game

000000c0  69 50 61 64 31 33 2c 31 30 01 03 00 00 00 05 66  iPad13,10......f
000000d0  72 2d 43 48 00 00 00 04 31 36 2e 32 00 00 00 00  r-CH....16.2....
//the device model, current country / region and the iOS version

000000e0  00 00 00 00 00 00 00 00 00 24 44 35 42 43 32 45  .........$D5BC2E
000000f0  37 30 2d 42 46 44 39 2d 34 38 33 36 2d 42 44 42  70-BFD9-4836-BDB
00000100  44 2d 33 33 32 34 33 42 35 30 45 30 36 38 00 00  D-33243B50E068..
//bundle ID or something ?

00000110  05 a8 01 00 00 00 06 35 37 2e 33 32 35 00 00 00  .......57.325...
//game version

00000320  00 00 00 00 00 00 00 00 00 03 46 52 41 00 00 00  ..........FRA...
//country ?

00000330  06 31 34 33 34 34 32                             .143442
//maybe an ID
//end of "readable" analyse of first req, back later to analyse / understand unreadable things
```
[the render request](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-002.txt)  
``` text
Analyse :
//the request look like an evaluation of performances, render e.g

000001d0  04 31 36 2e 32 00 00 00 09 69 50 61 64 31 33 2c  .16.2....iPad13,
000001e0  31 30 00 00 00 00                                10....
//iOS version, device model
```
[shop request 1](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-003.txt), [shop request 2](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-004.txt)
``` text
Analyse :
//requests used for the shop, req1 is auto, req2 when you open the shop

//req 1 = infos about transactions e.g
//req 2 = action (open the shop), and if you can buy items in the shop with money, if your game is an outstore installation, modified e.g, security don't let you buy with money I guess

00000040  68 6f 70 22 2c 22 70 72 6f 64 75 63 74 22 3a 22  hop","product":"
00000050  62 69 6c 6c 69 6e 67 4f 6b 22 2c 22 70 72 6f 64  billingOk","prod
//I can buy with money
```
[open news request 1](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-005.txt), [open news request 2](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-006.txt)  

[web API request ?](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-007.txt)  

[join training map request](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-008.txt)
``` text
Analyse :
"button":2 probably the choice if you wan't to be in training map or training match vs bots
"training_context":2 same
"hero":16000067 the static ID of your hero, here willow for example
"hero_owned":true if you have the hero
"skin":29000629 the static ID of your skin, here default willow
"skin_owned":true if you have the skin
"hero_level":11 the level of your hero
"gadget":23000564 the static ID of your gadget
"star_power":23000562 the static ID of your star power
"gear1":62000004 the static ID of your gear 1
"gear2":62000002 the static ID of your gear 2
```
[send text message in chat request](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-009.txt)
``` text
Analyse :
00000000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
00000010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
00000020  00 00 00 04 6c 6d 61 6f                          ....lmao
//the message that I send
```
[send emote in chat request 1](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-010.txt), [send emote in chat request 2](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-011.txt)
``` text
Analyse :
//request emote 1
00000000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
00000010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
00000020  28 2e 00 00 88 f0 cb 31 00                       (......1.

//request emote 2
00000000  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
00000010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
00000020  28 2e 00 00 89 f0 cb 31 00                       (......1.

//here emotes ID are 0x88 and 0x89, it's the place in a list (0x88=brawl stars emote, 0x89=numbers emote):
```
![alt text](https://raw.githubusercontent.com/slayy2357/mimi/refs/heads/main/requests/pictures/2.png)
[game/screen status request 1](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-012.txt), [game/screen status request 2](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-013.txt)
``` text
Analyse :
//request 1
00000020  00 00 00 09 6c 61 73 65 72 5f 66 70 73 00 00 00  ....laser_fps...
00000030  2e 7b 22 66 70 73 22 3a 35 37 2c 22 6d 6f 64 65  .{"fps":57,"mode
00000040  22 3a 22 69 6e 67 61 6d 65 22 2c 22 6c 6f 63 61  ":"ingame","loca
00000050  74 69 6f 6e 22 3a 31 35 30 30 30 37 37 36 7d     tion":15000776}

//request 2
00000020  00 00 00 09 6c 61 73 65 72 5f 66 70 73 00 00 00  ....laser_fps...
00000030  18 7b 22 66 70 73 22 3a 35 39 2c 22 6d 6f 64 65  .{"fps":59,"mode
00000040  22 3a 22 6d 65 6e 75 22 7d                       ":"menu"}

//request 1 when you are in game and switch back to menu mode
//request 2 when you are in menu and switch to game mode (black borders maybe)
```
[all tutorial requests](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-014.txt)  
[catalog request](https://github.com/slayy2357/mimi/blob/main/requests/dump/decrypted-015.txt)

### Payloads modifications
I also tried modifying different requests, and everything worked: changing the chosen hero, modifying the chat message, chat emote, ingame emotes, e.g  

I'm still working on the player position payload, rewards, etc.

## Responses from server

### Different types of responses
- Internal : the response will only be handled by the internal functions of the game --> this is what we are looking for  
- External : the response will only be handled by external functions, like dylibs etc. They are more "low-level" and less relevant to what we are looking for

### Crypted response analyse
Crypted response example : [crypted-002.txt](https://github.com/slayy2357/mimi/blob/main/requests/dump/crypted-002.txt)

### Decrypting internal responses
For responses, it's a bit different compared to send requests, because there are several functions for different things. For example, there's an 'in-game responses' function (only for coordinates, etc.) and a 'menu responses' function, etc.

At the moment, I can only get in-game responses and decrypt them, not menu responses etc, as I haven't had the time to work on that yet—but I will later

### Decrypted internal responses analyse