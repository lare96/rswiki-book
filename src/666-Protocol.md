\[\[Category RS2\]\] This page is an attempt to document the protocol
and other relevant information for client build 666.

== '''Handshakes''' == This section documents the handshake packets sent
from the client to the server.

  {
  ---

! Name ! Opcode ! Length ! Fields ! Description \|- ! Init Game
Connection \| 14 \| 0 \| None \| Indicates that the connection is for
logging the client into the lobby or game. \|- ! Init Update Connection
\| 15 \| 4 \| style="text-align: left" \| \* client\_build: int32 \|
Indicates that the connection is for streaming the game's resources to
the client. \|- ! Game Login \| 16 \| Variable (short) \|
style="text-align: left" \| \* client\_build: int32 \* reconnecting:
int8 \* rsa\_block\_len: int16 \* RSA block \*\* block\_header: int8
\*\* xtea\_key: int32\[4\] \*\* unknown: int64 \*\* password: cstring
\*\* server\_session\_key: int64 \*\* client\_session\_key: int64 \*
XTEA block \*\* username: cstring \*\* unknown: int8 \*\* screen\_type:
int8 \*\* screen\_width: int16 \*\* screen\_height: int16 \*\*
multisampling\_level: int8 \*\* uid: int8\[24\] \*\* settings: cstring
\*\* affiliate\_id: int32 \*\* preferences\_length: int8 \*\*
preferences\_data: int8\[preferences\_length\] \*\* System info \*\*\*
sysinfo\_version: int8 \*\*\* os\_type: int8 \*\*\* is\_64bit: int8
\*\*\* os\_version: int8 \*\*\* java\_vendor: int8 \*\*\* java\_release:
int8 \*\*\* java\_version: int8 \*\*\* java\_update: int8 \*\*\*
unsigned: int8 \*\*\* heap\_size: int16 \*\*\* processor\_count: int8
\*\*\* total\_memory: int24 \*\*\* unknown: int16 \*\*\* unknown: int8
\*\*\* unknown: int8 \*\*\* unknown: int8 \*\*\* unknown: jstring \*\*\*
unknown: jstring \*\*\* unknown: jstring \*\*\* unknown: jstring \*\*\*
unknown: int8 \*\*\* unknown: int16 \*\* unknown: int32 \*\* user\_flow:
int64 \*\* has\_additional\_info: int8 \*\*\* additional\_info: cstring
\*\* has\_jagtheora: int8 \*\* using\_javascript: int8 \*\*
archive\_checksums: int32\[36\] \| Attempts to login to the game server.
\|- ! Handshake 17 \| 17 \| 0 \| None \| Not used but present in the
client. \|- ! Lobby Login \| 19 \| Variable (short) \|
style="text-align: left" \| \* client\_build: int32 \* rsa\_block\_len:
int16 \* RSA block \*\* block\_header: int8 \*\* xtea\_key: int32\[4\]
\*\* unknown: int64 \*\* password: cstring \*\* server\_session\_key:
int64 \*\* client\_session\_key: int64 \* XTEA block \*\* username:
cstring \*\* game\_id: int8 \*\* language: int8 \*\* uid: int8\[24\]
\*\* settings: cstring \*\* affiliate\_id: int32 \*\*
archive\_checksums: int32\[36\] \| Attempts to login to the lobby
server. \|- ! Create Account \| 22 \| Variable (short) \|
style="text-align: left" \| \* client\_build: int32 \* rsa\_block\_len:
int16 \* RSA block \*\* block\_header: int8 \*\* xtea\_key: int32\[4\]
\*\* padding: int32\[10\] \*\* unknown: int16 \* XTEA block \*\* email:
cstring \*\* affiliate\_id: int16 \*\* password: cstring \*\*
user\_flow: int64 \*\* language: int8 \*\* game\_id: int8 \*\* uid:
int8\[24\] \*\* has\_additional\_info: int8 \*\*\* additional\_info:
cstring \*\* age: int8 \*\* email\_updates: int8 \*\* padding: int8\[7\]
\| Attempts to create an account with the given information. \|- !
Handshake 23 \| 23 \| 4 \| None \| Not used but present in the client.
\|- ! Handshake 24 \| 24 \| Variable (byte) \| None \| Not used but
present in the client. \|- ! Finished Advertisement \| 26 \| 0 \| None
\| Notifies the server that the client has finished viewing the
advertisement. \|- ! Handshake 27 \| 27 \| 0 \| None \| Not used but
present in the client. \|- ! Check Email \| 28 \| Variable (short) \|
style="text-align: left" \| \* client\_build: int32 \* rsa\_block\_len:
int16 \* RSA block \*\* block\_header: int8 \*\* xtea\_key: int32\[4\]
\*\* padding: int32\[10\] \*\* unknown: int16 \* XTEA block \*\* email:
cstring \*\* language: int8 \*\* padding: int8\[7\] \| Asks the server
to verify whether the given email can be used to create an account. \|-
! Init Social Network Connection \| 29 \| Variable (short) \|
style="text-align: left" \| \* client\_build: int32 \* reconnecting:
int8 (if game server connection) \* rsa\_block\_len: int16 \* RSA block
\*\* block\_header: int8 \*\* xtea\_key: int32\[4\] \*\*
social\_network\_id: int8 \*\* unknown: int16 \*\* language: int8 \*\*
affiliate\_id: int32 \*\* padding: int32\[6\] \*\* client\_session\_key:
int64 \*\* game\_id: int8 \*\* unknown: int8 \| Initializes a connection
using a social network. \|- ! Social Network Login \| 30 \| Variable
(short) \| style="text-align: left" \| \* Game login \*\* XTEA block
\*\*\* unknown: int8 \*\*\* screen\_type: int8 \*\*\* screen\_width:
int16 \*\*\* screen\_height: int16 \*\*\* multisampling\_level: int8
\*\*\* uid: int8\[24\] \*\*\* settings: cstring \*\*\* affiliate\_id:
int32 \*\*\* preferences\_length: int8 \*\*\* preferences\_data:
int8\[preferences\_length\] \*\*\* System info \*\*\*\*
sysinfo\_version: int8 \*\*\*\* os\_type: int8 \*\*\*\* is\_64bit: int8
\*\*\*\* os\_version: int8 \*\*\*\* java\_vendor: int8 \*\*\*\*
java\_release: int8 \*\*\*\* java\_version: int8 \*\*\*\* java\_update:
int8 \*\*\*\* unsigned: int8 \*\*\*\* heap\_size: int16 \*\*\*\*
processor\_count: int8 \*\*\*\* total\_memory: int24 \*\*\*\* unknown:
int16 \*\*\*\* unknown: int8 \*\*\*\* unknown: int8 \*\*\*\* unknown:
int8 \*\*\*\* unknown: jstring \*\*\*\* unknown: jstring \*\*\*\*
unknown: jstring \*\*\*\* unknown: jstring \*\*\*\* unknown: int8
\*\*\*\* unknown: int16 \*\*\* unknown: int32 \*\*\* user\_flow: int64
\*\*\* has\_additional\_info: int8 \*\*\*\* additional\_info: cstring
\*\*\* has\_jagtheora: int8 \*\*\* using\_javascript: int8 \*\*\*
archive\_checksums: int32\[36\] \* Lobby login \*\* XTEA block \*\*\*
game\_id: int8 \*\*\* language: int8 \*\*\* uid: int8\[24\] \*\*\*
settings: cstring \*\*\* affiliate\_id: int32 \*\*\* archive\_checksums:
int32\[36\] \| Attempts to login to either the game or lobby server
using a social network. \|- \|}

== '''Game Protocol''' ==

=== '''Packets''' === This section documents the packets sent between
the client and server during normal gameplay.

==== '''Client-to-Server''' ==== {\| class="wikitable"
style="text-align: center" \|- ! Name ! Opcode ! Length ! Fields !
Description \|- ! Map Build Complete \| 0 \| 0 \| None \| Indicates that
the client has just finished rebuilding its map. \|- ! Location Option 1
\| 1 \| 7 \| style="text-align: left" \| \* pos\_y: int16a \* pos\_x:
le\_int16a \* location\_id: le\_int16 \* ctrl\_pressed: int8 \| Sent
when the first option for a location is selected. \|- ! Remove Ignore \|
2 \| Variable (byte) \| style="text-align: left" \| \* name: cstring \|
Sent when the client wants to stop ignoring a player. \|- ! Packet 3 \|
3 \| 8 \| \| \|- ! Player Option 8 \| 4 \| 3 \| style="text-align: left"
\| \* player\_index: int16 \* ctrl\_pressed: int8 \| Sent when the
eighth option for a player is selected. \|- ! Packet 5 \| 5 \| Variable
(byte) \| \| \|- ! Packet 6 \| 6 \| 15 \| \| \|- ! Packet 7 \| 7 \| 8 \|
\| \|- ! Packet 8 \| 8 \| 6 \| \| \|- ! Packet 9 \| 9 \| Variable (byte)
\| \| \|- ! Npc Option 3 \| 10 \| 3 \| style="text-align: left" \| \*
npc\_index: le\_int16a \* ctrl\_pressed: int8c \| Sent when the third
option for an npc is selected. \|- ! Packet 11 \| 11 \| 8 \| \| \|- !
Packet 12 \| 12 \| Variable (byte) \| \| \|- ! Packet 13 \| 13 \|
Variable (byte) \| \| \|- ! Player Option 10 \| 14 \| 3 \|
style="text-align: left" \| \* player\_index: int16 \* ctrl\_pressed:
int8 \| Sent when the tenth option for a player is selected. \|- !
Packet 15 \| 15 \| 4 \| \| \|- ! Ground Object Option 5 \| 16 \| 7 \|
style="text-align: left" \| \* pos\_y: int16 \* object\_id: int16 \*
pos\_x: int16 \* ctrl\_pressed: int8 \| Sent when the fifth option for a
ground object is selected. \|- ! Packet 17 \| 17 \| 8 \| \| \|- ! Packet
18 \| 18 \| 1 \| \| \|- ! Packet 19 \| 19 \| Variable (byte) \| \| \|- !
Packet 20 \| 20 \| 4 \| \| \|- ! Packet 21 \| 21 \| 2 \| \| \|- ! Packet
22 \| 22 \| Variable (byte) \| \| \|- ! Ground Object Option 2 \| 23 \|
7 \| style="text-align: left" \| \* pos\_y: int16 \* object\_id: int16
\* pos\_x: int16 \* ctrl\_pressed: int8 \| Sent when the second option
for a ground object is selected. \|- ! Ground Object Option 3 \| 24 \| 7
\| style="text-align: left" \| \* pos\_y: int16 \* object\_id: int16 \*
pos\_x: int16 \* ctrl\_pressed: int8 \| Sent when the third option for a
ground object is selected. \|- ! Packet 25 \| 25 \| 8 \| \| \|- ! Packet
26 \| 26 \| 16 \| \| \|- ! Npc Option 6 \| 27 \| 3 \| style="text-align:
left" \| \* npc\_index: le\_int16a \* ctrl\_pressed: int8c \| Sent when
the sixth option for an npc is selected. \|- ! Ground Object Option 6 \|
28 \| 7 \| style="text-align: left" \| \* pos\_y: int16 \* object\_id:
int16 \* pos\_x: int16 \* ctrl\_pressed: int8 \| Sent when the sixth
option for a ground object is selected. \|- ! Npc Option 1 \| 29 \| 3 \|
style="text-align: left" \| \* npc\_index: le\_int16a \* ctrl\_pressed:
int8c \| Sent when the first option for an npc is selected. \|- ! Packet
30 \| 30 \| Variable (byte) \| \| \|- ! Add Friend \| 31 \| Variable
(byte) \| style="text-align: left" \| \* name: cstring \| Sent when the
client wants to be friends with a player. \|- ! Detect Modified Client
\| 32 \| 4 \| style="text-align: left" \| \* value: int32 \| Sent on
each map rebuild if the client detects that it is not running as an
applet. The ''value'' field will always have value 0x3f008edd. \|- ! No
Timeout \| 33 \| 0 \| None \| Sent if the client has not sent the server
any data for 50 ticks. \|- ! Packet 34 \| 34 \| 6 \| \| \|- ! Remove
Friend \| 35 \| Variable (byte) \| style="text-align: left" \| \* name:
cstring \| Sent when the client wants to stop being friends with a
player. \|- ! Packet 36 \| 36 \| 6 \| \| \|- ! Packet 37 \| 37 \| 4 \|
\| \|- ! Location Option 5 \| 38 \| 7 \| style="text-align: left" \| \*
pos\_y: int16a \* pos\_x: le\_int16a \* location\_id: le\_int16 \*
ctrl\_pressed: int8 \| Sent when the fifth option for a location is
selected. \|- ! Location Option 2 \| 39 \| 7 \| style="text-align: left"
\| \* pos\_y: int16a \* pos\_x: le\_int16a \* location\_id: le\_int16 \*
ctrl\_pressed: int8 \| Sent when the second option for a location is
selected. \|- ! Packet 40 \| 40 \| 8 \| \| \|- ! Packet 41 \| 41 \| 12
\| \| \|- ! Packet 42 \| 42 \| 15 \| \| \|- ! Player Option 1 \| 43 \| 3
\| style="text-align: left" \| \* player\_index: int16 \* ctrl\_pressed:
int8 \| Sent when the first option for a player is selected. \|- !
Player Option 2 \| 44 \| 3 \| style="text-align: left" \| \*
player\_index: int16 \* ctrl\_pressed: int8 \| Sent when the second
option for a player is selected. \|- ! Ground Object Option 1 \| 45 \| 7
\| style="text-align: left" \| \* pos\_y: int16 \* object\_id: int16 \*
pos\_x: int16 \* ctrl\_pressed: int8 \| Sent when the first option for a
ground object is selected. \|- ! Packet 46 \| 46 \| Variable (byte) \|
\| \|- ! Player Option 5 \| 47 \| 3 \| style="text-align: left" \| \*
player\_index: int16 \* ctrl\_pressed: int8 \| Sent when the fifth
option for a player is selected. \|- ! Packet 48 \| 48 \| 8 \| \| \|- !
Ground Object Option 4 \| 49 \| 7 \| style="text-align: left" \| \*
pos\_y: int16 \* object\_id: int16 \* pos\_x: int16 \* ctrl\_pressed:
int8 \| Sent when the fourth option for a ground object is selected. \|-
! Packet 50 \| 50 \| Variable (byte) \| \| \|- ! Player Option 9 \| 51
\| 3 \| style="text-align: left" \| \* player\_index: int16 \*
ctrl\_pressed: int8 \| Sent when the ninth option for a player is
selected. \|- ! Packet 52 \| 52 \| 4 \| \| \|- ! Minimap Walk \| 53 \|
18 \| style="text-align: left" \| \* dest\_y: le\_int16 \*
ctrl\_pressed: int8c \* dest\_x: le\_int16a \* dummy1: int8 \* dummy2:
int8 \* camera\_yaw: int16 \* dummy3: int8 \* yaw\_random: int8 \*
scale\_random: int8 \* dummy4: int8 \* world\_x: int16 \* world\_y:
int16 \* dummy5: int8 \| Sent when a position on the minimap is clicked.
Fields ''dummy1'' and ''dummy2'' should have value -1. ''dummy3'' should
have value 57. ''dummy4'' should have value 89. ''dummy5'' should have
value 63.

The ''yaw\_random'' and ''scale\_random'' contain the current random
offsets added to the client's camera yaw and minimap scale values.

The ''world\_x'' and ''world\_y'' fields contain the client's current
position in world coordinates. \|- ! Packet 54 \| 54 \| 8 \| \| \|- !
Packet 55 \| 55 \| Variable (byte) \| \| \|- ! Game View Walk \| 56 \| 5
\| style="text-align: left" \| \* dest\_y: le\_int16 \* ctrl\_pressed:
int8c \* dest\_x: le\_int16a \| Sent when a position in the 3d game
world is clicked. \|- ! Packet 57 \| 57 \| 11 \| \| \|- ! Location
Option 4 \| 58 \| 7 \| style="text-align: left" \| \* pos\_y: int16a \*
pos\_x: le\_int16a \* location\_id: le\_int16 \* ctrl\_pressed: int8 \|
Sent when the fourth option for a location is selected. \|- ! Packet 59
\| 59 \| Variable (byte) \| \| \|- ! Packet 60 \| 60 \| 1 \| \| \|- !
Npc Option 5 \| 61 \| 3 \| style="text-align: left" \| \* npc\_index:
le\_int16a \* ctrl\_pressed: int8c \| Sent when the fifth option for an
npc is selected. \|- ! Packet 62 \| 62 \| Variable (byte) \| \| \|- !
Packet 63 \| 63 \| 4 \| \| \|- ! Packet 64 \| 64 \| 0 \| \| \|- ! Packet
65 \| 65 \| 11 \| \| \|- ! Packet 66 \| 66 \| 8 \| \| \|- ! Packet 67 \|
67 \| 2 \| \| \|- ! Add Ignore \| 68 \| Variable (byte) \|
style="text-align: left" \| \* name: cstring \* temporary: int8 \| Sent
when the client wants to ignore a player. \|- ! Npc Option 4 \| 69 \| 3
\| style="text-align: left" \| \* npc\_index: le\_int16a \*
ctrl\_pressed: int8c \| Sent when the fourth option for an npc is
selected. \|- ! Npc Option 2 \| 70 \| 3 \| style="text-align: left" \|
\* npc\_index: le\_int16a \* ctrl\_pressed: int8c \| Sent when the
second option for an npc is selected. \|- ! Packet 71 \| 71 \| 16 \| \|
\|- ! Player Option 7 \| 72 \| 3 \| style="text-align: left" \| \*
player\_index: int16 \* ctrl\_pressed: int8 \| Sent when the seventh
option for a player is selected. \|- ! Packet 73 \| 73 \| 2 \| \| \|- !
Packet 74 \| 74 \| Variable (byte) \| \| \|- ! Location Option 6 \| 75
\| 7 \| style="text-align: left" \| \* pos\_y: int16a \* pos\_x:
le\_int16a \* location\_id: le\_int16 \* ctrl\_pressed: int8 \| Sent
when the sixth option for a location is selected. \|- ! Packet 76 \| 76
\| 4 \| \| \|- ! Packet 77 \| 77 \| 2 \| \| \|- ! Packet 78 \| 78 \| 3
\| \| \|- ! Packet 79 \| 79 \| Variable (byte) \| \| \|- ! Packet 80 \|
80 \| Variable (byte) \| \| \|- ! Packet 81 \| 81 \| Variable (byte) \|
\| \|- ! Packet 82 \| 82 \| Variable (byte) \| \| \|- ! Player Option 4
\| 83 \| 3 \| style="text-align: left" \| \* player\_index: int16 \*
ctrl\_pressed: int8 \| Sent when the fourth option for a player is
selected. \|- ! Packet 84 \| 84 \| 8 \| \| \|- ! Packet 85 \| 85 \| 8 \|
\| \|- ! Location Option 3 \| 86 \| 7 \| style="text-align: left" \| \*
pos\_y: int16a \* pos\_x: le\_int16a \* location\_id: le\_int16 \*
ctrl\_pressed: int8 \| Sent when the third option for a location is
selected. \|- ! Packet 87 \| 87 \| 0 \| \| \|- ! Packet 88 \| 88 \|
Variable (byte) \| \| \|- ! Packet 89 \| 89 \| Variable (byte) \| \| \|-
! Player Option 3 \| 90 \| 3 \| style="text-align: left" \| \*
player\_index: int16 \* ctrl\_pressed: int8 \| Sent when the third
option for a player is selected. \|- ! Player Option 6 \| 91 \| 3 \|
style="text-align: left" \| \* player\_index: int16 \* ctrl\_pressed:
int8 \| Sent when the sixth option for a player is selected. \|- !
Packet 92 \| 92 \| 4 \| \| \|- ! Packet 93 \| 93 \| Variable (byte) \|
\| \|- \|}

==== '''Server-to-Client''' ==== {\| class="wikitable"
style="text-align: center" \|- ! Name ! Opcode ! Length ! Fields !
Description \|- ! Packet 0 \| 0 \| 2 \| \| \|- ! Packet 1 \| 1 \| 0 \|
\| \|- ! Packet 2 \| 2 \| Variable (byte) \| \| \|- ! Packet 3 \| 3 \|
Variable (byte) \| \| \|- ! Packet 4 \| 4 \| 0 \| \| \|- ! Packet 5 \| 5
\| Variable (short) \| \| \|- ! Packet 6 \| 6 \| Variable (short) \| \|
\|- ! Packet 7 \| 7 \| 20 \| \| \|- ! Packet 8 \| 8 \| 6 \| \| \|- !
Packet 9 \| 9 \| 10 \| \| \|- ! Packet 10 \| 10 \| 2 \| \| \|- ! Packet
11 \| 11 \| Variable (short) \| \| \|- ! Packet 12 \| 12 \| 7 \| \| \|-
! Packet 13 \| 13 \| Variable (short) \| \| \|- ! Packet 14 \| 14 \| 6
\| \| \|- ! Packet 15 \| 15 \| Variable (byte) \| \| \|- ! Packet 16 \|
16 \| 2 \| \| \|- ! Packet 17 \| 17 \| Variable (byte) \| \| \|- !
Packet 18 \| 18 \| 1 \| \| \|- ! Packet 19 \| 19 \| Variable (short) \|
\| \|- ! Packet 20 \| 20 \| 4 \| \| \|- ! Packet 21 \| 21 \| 3 \| \| \|-
! Packet 22 \| 22 \| 6 \| \| \|- ! Packet 23 \| 23 \| Variable (short)
\| \| \|- ! Packet 24 \| 24 \| 5 \| \| \|- ! Packet 25 \| 25 \| 17 \| \|
\|- ! Packet 26 \| 26 \| 6 \| \| \|- ! Packet 27 \| 27 \| 16 \| \| \|- !
Packet 28 \| 28 \| 0 \| \| \|- ! Packet 29 \| 29 \| 4 \| \| \|- ! Packet
30 \| 30 \| 3 \| \| \|- ! Packet 31 \| 31 \| 10 \| \| \|- ! Packet 32 \|
32 \| Variable (short) \| \| \|- ! Packet 33 \| 33 \| 6 \| \| \|- !
Packet 34 \| 34 \| 6 \| \| \|- ! Packet 35 \| 35 \| 7 \| \| \|- ! Packet
36 \| 36 \| Variable (byte) \| \| \|- ! Packet 37 \| 37 \| 6 \| \| \|- !
Packet 38 \| 38 \| Variable (short) \| \| \|- ! Packet 39 \| 39 \| 6 \|
\| \|- ! Packet 40 \| 40 \| 9 \| \| \|- ! Packet 41 \| 41 \| Variable
(short) \| \| \|- ! Packet 42 \| 42 \| 12 \| \| \|- ! Packet 43 \| 43 \|
2 \| \| \|- ! Packet 44 \| 44 \| 1 \| \| \|- ! Packet 45 \| 45 \|
Variable (byte) \| \| \|- ! Packet 46 \| 46 \| 3 \| \| \|- ! Packet 47
\| 47 \| 8 \| \| \|- ! Packet 48 \| 48 \| 4 \| \| \|- ! Packet 49 \| 49
\| 3 \| \| \|- ! Packet 50 \| 50 \| Variable (byte) \| \| \|- ! Packet
51 \| 51 \| 4 \| \| \|- ! Packet 52 \| 52 \| Variable (byte) \| \| \|- !
Packet 53 \| 53 \| 0 \| \| \|- ! Packet 54 \| 54 \| Variable (byte) \|
\| \|- ! Packet 55 \| 55 \| 3 \| \| \|- ! Packet 56 \| 56 \| Variable
(short) \| \| \|- ! Packet 57 \| 57 \| 8 \| \| \|- ! Packet 58 \| 58 \|
2 \| \| \|- ! Packet 59 \| 59 \| 0 \| \| \|- ! Packet 60 \| 60 \| 4 \|
\| \|- ! Packet 61 \| 61 \| 6 \| \| \|- ! Packet 62 \| 62 \| Variable
(byte) \| \| \|- ! Packet 63 \| 63 \| Variable (byte) \| \| \|- ! Packet
64 \| 64 \| 10 \| \| \|- ! Packet 65 \| 65 \| 3 \| \| \|- ! Packet 66 \|
66 \| 4 \| \| \|- ! Packet 67 \| 67 \| 4 \| \| \|- ! Packet 68 \| 68 \|
0 \| \| \|- ! Packet 69 \| 69 \| Variable (short) \| \| \|- ! Packet 70
\| 70 \| Variable (short) \| \| \|- ! Packet 71 \| 71 \| Variable (byte)
\| \| \|- ! Packet 72 \| 72 \| 8 \| \| \|- ! Packet 73 \| 73 \| 3 \| \|
\|- ! Packet 74 \| 74 \| 6 \| \| \|- ! Packet 75 \| 75 \| 4 \| \| \|- !
Packet 76 \| 76 \| 7 \| \| \|- ! Packet 77 \| 77 \| 3 \| \| \|- ! Packet
78 \| 78 \| 4 \| \| \|- ! Packet 79 \| 79 \| 1 \| \| \|- ! Packet 80 \|
80 \| 6 \| \| \|- ! Packet 81 \| 81 \| 10 \| \| \|- ! Packet 82 \| 82 \|
Variable (short) \| \| \|- ! Packet 83 \| 83 \| 1 \| \| \|- ! Packet 84
\| 84 \| Variable (short) \| \| \|- ! Packet 85 \| 85 \| 0 \| \| \|- !
Packet 86 \| 86 \| Variable (short) \| \| \|- ! Packet 87 \| 87 \| 8 \|
\| \|- ! Packet 88 \| 88 \| Variable (byte) \| \| \|- ! Packet 89 \| 89
\| 8 \| \| \|- ! Packet 90 \| 90 \| Variable (byte) \| \| \|- ! Packet
91 \| 91 \| 10 \| \| \|- ! Packet 92 \| 92 \| 28 \| \| \|- ! Packet 93
\| 93 \| 12 \| \| \|- ! Packet 94 \| 94 \| 0 \| \| \|- ! Packet 95 \| 95
\| Variable (short) \| \| \|- ! Packet 96 \| 96 \| Variable (byte) \| \|
\|- ! Packet 97 \| 97 \| 8 \| \| \|- ! Packet 98 \| 98 \| Variable
(byte) \| \| \|- ! Packet 99 \| 99 \| Variable (short) \| \| \|- !
Packet 100 \| 100 \| 3 \| \| \|- ! Packet 101 \| 101 \| 4 \| \| \|- !
Packet 102 \| 102 \| 5 \| \| \|- ! Packet 103 \| 103 \| 0 \| \| \|- !
Packet 104 \| 104 \| 1 \| \| \|- ! Packet 105 \| 105 \| 6 \| \| \|- !
Packet 106 \| 106 \| Variable (byte) \| \| \|- ! Packet 107 \| 107 \| 5
\| \| \|- ! Packet 108 \| 108 \| 9 \| \| \|- ! Packet 109 \| 109 \| 6 \|
\| \|- ! Packet 110 \| 110 \| 2 \| \| \|- ! Packet 111 \| 111 \| 3 \| \|
\|- ! Packet 112 \| 112 \| Variable (short) \| \| \|- ! Packet 113 \|
113 \| 1 \| \| \|- ! Packet 114 \| 114 \| Variable (byte) \| \| \|- !
Packet 115 \| 115 \| 6 \| \| \|- ! Packet 116 \| 116 \| 12 \| \| \|- !
Packet 117 \| 117 \| Variable (byte) \| \| \|- ! Packet 118 \| 118 \|
Variable (byte) \| \| \|- ! Packet 119 \| 119 \| 11 \| \| \|- ! Packet
120 \| 120 \| 6 \| \| \|- ! Packet 121 \| 121 \| 4 \| \| \|- ! Packet
122 \| 122 \| Variable (short) \| \| \|- ! Packet 123 \| 123 \| 3 \| \|
\|- ! Packet 124 \| 124 \| 7 \| \| \|- ! Packet 125 \| 125 \| 0 \| \|
\|- ! Packet 126 \| 126 \| 0 \| \| \|- ! Packet 127 \| 127 \| Variable
(byte) \| \| \|- ! Packet 128 \| 128 \| 0 \| \| \|- ! Packet 129 \| 129
\| 2 \| \| \|- ! Packet 130 \| 130 \| Variable (byte) \| \| \|- ! Packet
131 \| 131 \| 6 \| \| \|- ! Packet 132 \| 132 \| 5 \| \| \|- ! Packet
133 \| 133 \| 6 \| \| \|- ! Packet 134 \| 134 \| Variable (short) \| \|
\|- ! Packet 135 \| 135 \| 6 \| \| \|- ! Packet 136 \| 136 \| 6 \| \|
\|- ! Packet 137 \| 137 \| 2 \| \| \|- ! Packet 138 \| 138 \| Variable
(short) \| \| \|- ! Packet 139 \| 139 \| 7 \| \| \|- ! Packet 140 \| 140
\| 1 \| \| \|- ! Packet 141 \| 141 \| Variable (short) \| \| \|- !
Packet 142 \| 142 \| 10 \| \| \|- ! Packet 143 \| 143 \| 6 \| \| \|- !
Packet 144 \| 144 \| Variable (byte) \| \| \|- \|}

== '''Update Protocol''' ==

This section documents the communication between the client and server
over the connection used to stream resources.

=== '''Handshake Response''' ===

After receiving a handshake for the update protocol, the server responds
with one of the following packets:

  {
  ---

! Name ! Id ! Fields ! Description \|- ! OK \| 0 \| style="text-align:
left" \| \* required\_resource\_sizes: int32\[27\] \| Indicates a
successful connection. \|- ! OUT\_OF\_DATE \| 6 \| None \| Indicates
that the client is outdated. \|- ! SERVER\_FULL \| 7 \| None \|
Indicates that the server is full at the moment. \|- ! IP\_LIMIT \| 9 \|
None \| Indicates that the client is being rate limited. \|- \|}

=== '''Packets''' === This section documents the packets sent between
the client and server over the update connection.

==== '''Client-to-Server''' ==== All packets sent by the client are 4
bytes long. Each packet includes a 1-byte opcode and a 3-byte payload.

  {
  ---

! Name ! Opcode ! Fields ! Description \|- ! Prefetch Request \| 0 \|
style="text-align: left" \| \* index: int8 \* file: int16 \| A passive
request for a resource. \|- ! Urgent Request \| 1 \| style="text-align:
left" \| \* index: int8 \* file: int16 \| An urgent request for a
resource. \|- ! Client Logged In \| 2 \| style="text-align: left" \| \*
padding: int24 \| Indicates that the client has logged in. May be useful
for adjusting response rate. \|- ! Client Logged Out \| 3 \|
style="text-align: left" \| \* padding: int24 \| Indicates that the
client has logged out. May be useful for adjusting response rate. \|- !
Update XOR Code \| 4 \| style="text-align: left" \| \* xor\_code: int8
\* padding: int16 \| Proposes a code to be used to encrypt all traffic.
May be used to bypass firewalls or related software. \|- ! Connection
Information \| 6 \| style="text-align: left" \| \* version: int24 \|
Sent after a connection is established. The ''version'' field always has
the value 3. \|- ! Drop Request Queue \| 7 \| style="text-align: left"
\| \* padding: int24 \| Asks for currently pending requests to be
dropped by the server. This packet is restricted to administrators by
the client. \|- \|}

==== '''Server-to-Client''' ==== The server responds to the client's
requests for particular resources by sending back the (possibly
compressed) files. The data is in the following format:

  {
  ---

! Field ! Description \|- \| index: int8 \| The resource's index. \|- \|
file: int16 \| The resource's file number. \|- \| compression\_type:
int8 \| The compression type of the file. Can be 0 (uncompressed), 1
(compressed using BZIP2), or 2 (compressed using GZIP). \|- \|
file\_size: int32 \| The (possibly compressed) size of the file. \|- \|
uncompressed\_size: int32 \| The uncompressed size of the file. This is
only present if the file is compressed (i.e. the ''compression\_type''
field is set to 1 or 2). \|- \| data: int8\[file\_size\] \| The
(possibly compressed) file data. \|- \|}

Of particular note is that the response is grouped into 512-byte blocks.
For every block after the first, the first byte of the block '''must'''
be 0xff (decimal 255).

In addition, if the client has updated its XOR code to be nonzero, the
server must XOR each byte of the response with the chosen code before it
sends it to the client.
