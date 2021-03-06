\[\[Category Packet\]\] \[\[Category Packet 317\]\] \[\[Category RS2\]\]
== '''Packet structure''' == When the client sends a packet to the
server, the first byte encapsulates its
\[http://en.wikipedia.org/wiki/Opcode opcode\]. This specific opcode is
encrypted with a value generated by the
\[http://en.wikipedia.org/wiki/ISAAC\_(cipher) ISAAC\]
\[http://en.wikipedia.org/wiki/Pseudorandom\_number\_generator PRNG\]
seeded with a dynamically server generated key during the login block.
The server decrypts it and associates the opcode to the packet's
respective predefined size. If the packet does not contain a fixed size,
the opcode will be followed by either a byte or a word - varying per
packet - for its proper size. This is then followed by the
\[http://en.wikipedia.org/wiki/Payload\_(software) payload\].

A list of the 317 Packets may be found
\[http://rswiki.moparisthebest.com/index.php?title=Category:Packet:317
here\].

== '''Login Protocol Overview''' == Every connection to the main
'gateway' server sends a single byte of data, mostly well known as the
connection type. The connection type tells the main server which type of
connection you wish to initiate. The old engine list consists of: \*
Login request - connection type 14 \* Update - connection type 15 \* New
connection login - connection type 16 \* Reconnecting login - connection
type 18

The connection type we will cover in the following paragraphs is the
login connection type, 14. After the login handshake initiating
connection type, the client writes hash derived from the logging in
player's username. This is believed to help select the appropriate login
server. On successful handshake, the server sends back 8 ignored bytes.

<pre>long encodedUsername = TextUtils.encodeAsBase37Integer(username);
int usernameHash = (int) (encodedUsername >> 16 & 31L);
out.offset = 0;
out.writeByte(14); // Initiate connection type
out.writeByte(usernameHash);
in.queueBytes(2, out.payload);
for (int j = 0; j < 8; j++) in.read();</pre>
At this point, the client reads in one byte, called the status code. The
status code 0 is expected to start the login protocol correctly. If the
status code is 0, the client reads a long, dubbed by many as the server
session key. This is used to help generate a unique seed for the client
session's packet opcode masking. The client then stores two ints that
are the upper and lower ints of the client session key, which has the
same purpose as the server's key. The client then starts writing the
login block, which is RSA encrypted.

The login block starts with the byte 10, which is considered a magic
number. Following it is the client session key and server session key
longs. After the session keys, the session's UID (unique identifier or
user identifier) is written to the block. This is used to distinguish
between multiple sessions. Trailing behind the UID comes the client's
username and password written as modified C-strings that are rather
terminated with a 10 byte than a NUL byte. This block is then RSA
encrypted and stored for later use.

Now starts the login request packet. It starts off with a flag telling
the server whether or not the client is reconnecting or connecting for
the first time. The byte is 18 or 16, respectively. \[NOW CLASSIFIED AS
A CONNECTION TYPE\] Following is the size of the rest of the login
response packet, including the login block that trails at the end, to
tip the server how much data it should expect. Later comes the magic
number byte 255, and right behind it the client revision short. The
packet is just about crafted completely. A flag byte that represents if
the client is running in low memory or high memory modes is sent, and
after the 9 CRC32 checksums of the file system 0 basic archives (this
includes versionlist, media, config, etc.). To top it off, the RSA
encrypted login block is appended to the end and the packet is sent to
the server.

The ISAAC ciphers are seeded for packet opcode masking after adding 50
to each int of the session keys, and the status code is reread. This
finishes the login protocol.

== '''Login Protocol Breakdown''' == The login is comprised of four
stages in which the client and server switch in regards to which one is
reading and which one is writing. <br/> ===Variables:=== The login
process has a lot of variable data, compiled here is a list of the
variables and their different values. ====Name Hash==== A hash of the
player name, thought to be used to select an appropriate login server.
This has no use in current private servers.

====Server Session Key==== The server-session-key is one of two ciphers
used to encrypt the game protocol, using the ISAAC algorithms. <br/>
===="Data File Version"==== <!-- Colby --> The CRC checks for the cache
files. <br/>

====User ID==== A client-side randomly generated integer. This could be
used in reassigning sessions to players that have lost connection. It is
stored as a packed integer in a file named 'uid.dat' in the cache
directory. <br/>

====Username==== The username of the player, used to identify their
account. <br/> ====Password==== The password of the player account, used
so only they can log into their account. <br/> ====Client Session
Key==== The client-session-key is one of two ciphers used to encrypt the
game protocol, using the ISAAC algorithms. <br/> ====Connect Status====
The status of the connection. {\|border=2 ! Value ! Status \|- \| 16 \|
Signifies that the connection is new. \|- \| 18 \| Signifies that the
session is reconnecting a previously lost connection. \|- \|}

====Size==== The size of the unencrypted login packet, used to determine
how many bytes need to be read from the stream by the server. <br/>
====Client Version==== The memory-version of the game client.
{\|border=2 ! Value ! Status \|- \| 0 \| Signifies the client is a
low-memory client. \|- \| 1 \| Signifies that the client is a
high-memory client. \|- \|}

====CRC Values==== 9 4-byte values, Each containing the CRC of their
respective cache files. Used by the server to verify client is up to
date. <br/>

====Player Status==== The in-game player status - player, player
moderator, or administrator. <br/> {\|border=2 ! Value ! Status \|- \| 0
\| Signifies that this player is a normal player. \|- \| 1 \| Signifies
that this player is a player moderator. \|- \| 2 \| Signifies that this
player is an administrator. \|- \|}

====Flagged==== If set to 1, information about mouse movements etc. are
sent to the server. Suspected bot accounts are flagged. <br/>
====Response Codes==== At the beginning and end of the login procedure,
we send different values to the client to allow or deny a login. The
various values show different messages on the login box on the client or
do something internally. <br/> {\| border=2 ! Value ! Response \|- \| -1
\| Waits for 2000ms and tries again while counting failures. \|- \| 0 \|
Exchanges session keys, player name, password, etc. \|- \| 1 \| Waits
for 2000ms and tries again. \|- \| 2 \| Client made a successful login.
\|- \| 3 \| "Invalid username or password." \|- \| 4 \| "Your account
has been disabled. Please check your message-center for details." \|- \|
5 \| "Your account is already logged in. Please try again in 60 secs..."
\|- \| 6 \| "RuneScape has been updated! Please reload this page." \|-
\| 7 \| "This world is full. Please use a different world." \|- \| 8 \|
"Unable to connect. Login server offline." \|- \| 9 \| "Login limit
exceeded. Too many connections from your address." \|- \| 10 \| "Unable
to connect. Bad session ID." \|- \| 11 \| "Login server rejected
session. Please try again." \|- \| 12 \| "You need a members account to
login to this world. Please subscribe, or use a different world." \|- \|
13 \| "Could not complete login. Please try using a different world."
\|- \| 14 \| "The server is being updated. Please wait 1 minute and try
again." \|- \| 15 \| See the \[\[\#Regarding response code 15\|notes
below\]\]. \|- \| 16 \| "Login attempts exceeded. Please wait 1 minute
and try again." \|- \| 17 \| "You are standing in a members-only area.
To play on this world move to a free area first." \|- \| 20 \| "Invalid
loginserver requested. Please try using a different world." \|- \| 21 \|
"You have only just left another world. Your profile will be transferred
in: (number) seconds." \|- \| None of the above \| "Unexpected server
response. Please try using a different world." \|- \|}

==== Regarding response code 15 ==== On the server, players are not
unregistered for quite some time. This can be best witnessed when the
client forcefully closes the connection while in combat. If you're quick
enough before the player dies or kills the NPC, login attempts during
that time return that the account is already logged in. This probably
explains why the message says "try again in 60 seconds", and they just
reused the response when the player is truly logged in.

Going along with this "players aren't offline yet" idea, when the client
experiences some lag and performs a reconnect, it sends byte 18 as it's
\[\[\#Connect Status\|connection type\]\] to the server.

The server most likely saves this as a boolean (reconnect = var == 18;).
When the login is entirely validated, meaning the password's are okay
and the server isn't full, it can either send back the normal response,
2, or 15.

But why 15? If you look at the client code, you'll see that the chat
messages aren't cleared. If you've ever had a poor connection you've
noticed that your chat stays there upon a reconnect, and this is exactly
why. <!-- thanks to Colby for this contribution--> When you implement
response code 15 though, you do NOT need to send the "player status" or
the "flagged" bytes.

===Login Process:=== ====Stage 1: Client -\> Server==== {\| border=2 \|-
! Data Type ! Value \|- \| ubyte \| 14 \|- \| ubyte \| "name hash" \|-
\|} <br/>

====Stage 2: Server -\> Client==== {\| border=2 \|- ! Data Type ! Value
\|- \| byte \| 0 \|- \| byte \| 0 \|- \| byte \| 0 \|- \| byte \| 0 \|-
\| byte \| 0 \|- \| byte \| 0 \|- \| byte \| 0 \|- \| byte \| 0 \|- \|
byte \| "response code" \|- \| \[\[QWord\|long\]\] \| "server session
key" \|- \|} <br/>

====Stage 3: Client -\> Server==== {\| border=2 \|- ! Data Type ! Value
\|- \| byte \| "connect status" \|- \| byte \| "size" \|- \| byte \| 255
\|- \| \[\[Word\|short\]\] \| 317 \|- \| byte \| "client version" \|- \|
\[\[DWord\|int\]\] \| "crc values"\[0\] \|- \| \[\[DWord\|int\]\] \|
"crc values"\[1\] \|- \| \[\[DWord\|int\]\] \| "crc values"\[2\] \|- \|
\[\[DWord\|int\]\] \| "crc values"\[3\] \|- \| \[\[DWord\|int\]\] \|
"crc values"\[4\] \|- \| \[\[DWord\|int\]\] \| "crc values"\[5\] \|- \|
\[\[DWord\|int\]\] \| "crc values"\[6\] \|- \| \[\[DWord\|int\]\] \|
"crc values"\[7\] \|- \| \[\[DWord\|int\]\] \| "crc values"\[8\] \|- \|
byte \| 10 \|- \| \[\[QWord\|long\]\] \| "client session key" \|- \|
\[\[QWord\|long\]\] \| "server session key" \|- \| \[\[DWord\|int\]\] \|
"user id" \|- \| \[\[RS String\|RS String\]\] \| "username" \|- \|
\[\[RS String\|RS String\]\] \| "password" \|- \|} <br/>

====Stage 4: Server -\> Client==== {\| border=2 \|- ! Data Type ! Value
\|- \| byte \| "response code" \|- \| byte \| "player status" \|- \|
byte \| "flagged" \|- \|} <br/>

== '''Player Updating''' == The player updating process consists of 4
parts: \* a) Our player movement updates \* b) Other player movement
updates \* c) Player list updating \* c.a) Apperance updating \* c.b)
Location updating \* d) Player update block flag-based updates

'''Our player movement updates'''

The client begins by reading 1 bit. This bit tells the client whether or
not it is currently updating 'our player', or the player the client is
controlling. If it's not updating our player, it exits and goes onto
step b. If it is, it then reads 2 bits. The value is called the movement
update type. There are 4 recognized movement update types:

-   Type 0 basically tells the client there is nothing to update for our
    player, just add its index to the local updating list.

-   Type 1 tells the client you moved in one direction. The client reads
    3 bits, which represents the direction you moved in, and then 1 bit,
    which states whether further update is required. If so, it adds it
    to the updating list. This is used in walking.

-   Type 2 functions in much of the same way as its previous, only this
    time it reads two 3 bit values. The first represents the player's
    last direction, and the second it's current direction. Trailing
    behind it is also the 1 bit 'update required' flag as type 1. This
    is used in running.

-   Type 3 on the other hand is different. It reads in 2 bits which
    represents our player's plane, or its level of height, in the game
    world.

Only 0-3 inclusive are appropriate planes supported by the client. It
then reads 1 bit, which describes whether or not to clear the
awaiting-waypoint queue, basically to stop client from further queued
stepping, such as used in teleporting. After this, it reads the 'update
required' bit, and checks to see if further update is required. Directly
after, it reads two 7 bit quantities, representing the new relative X
and relative Y coordinates of our player to our current map region's
origin. It then sets our players position to the plane, x, and y
positions as told to.

'''Other player movement updates'''

The client begins by reading an 8 bit value telling the client how many
players there are to update. It then enters a loop for each player there
is to update.

Inside this loop, the client reads 1 bit. This is the movement update
required flag. If the flag is 0, it sets the current updating player's
last update cycle time to the current game logic loop cycle time, and
adds the player to the local player list. If the flag is not 0, it then
reads the movement update type, which is a 2 bit quantity. The following
known types are:

-   0, the client updates the current player's last update cycle time,
    adds the current player to the local player list, and adds it to the
    updating list.

-   1, the client updates the current player's last update cycle time
    and adds the current player to the local player list as well, but
    also reads in 3 bit quantity. This represents the current player's
    direction it walked to. It then reads the a 1 bit value that
    specifies whether or not to add the player to the updating list.

-   2, the client does the exact same thing as the type 2 update, except
    it reads in two of the 3 bit quantities. The first represents the
    current player's last direction, and the second its current
    direction running.

-   3, the client queues to remove the player from the list of players
    to be updated but it is possible such as in the instances for when
    players teleport to add them back to the list of players to be
    updated during the populate update.

'''Player list updating'''

The next step in the player updating procedure is the player list
updating, or where the client recieves data on every player in its local
list, such as appearance and location relative to ours. The client loops
through a process for each player in the updating.

The client reads an 11 bit quantity from the buffer, which is the next
player in the updated list to be informed about. The clietn then checks
if it has a cached buffer for that player's updating, and if it does, it
updates the player appearance.

'''Appearance updating'''

Appearance updating starts off by first reading an unsigned byte that
represents the current player's gender. Then it reads another unsigned
byte that represents the player's over-head icon id. This is used with
prayer icons above heads. Next, a loop occurs 12 times to read equipment
data.

In the loop, the client reads an unsigned byte that is the equipment
slot's item id high byte. If it is 0, the player's equpment slot has no
item. If it is not 0, another unsigned byte is read the merged with the
previous to create the equipment's item id. If the id is 65535 (written
as a -1 signed short), then the player's appearance is that of an NPC.
The client reads in an unsigned short representing the NPC's id and sets
the player's definition to that NPC's.

After the equipment loop, it loops 5 times, once for each type of
coloured body part. In each loop, the client reads an unsigned short and
assigns it as the color of the current loop idx (which represents the
body part).

Finally, after the color loop, the client reads 7 unsigned shorts
representing animation indices; the animations belong to: \* Standing
still \* Turning while standing \* Walking \* Turning around (backwards)
\* Turning a quarter-way clockwise \* Turning a quarter-way counter
clockwise \* Running

After these animation indices are read, a long representing the player's
name is read, an unsigned byte representing the combat level, and an
unsigned short representing the players skill level (for things where
players arent ranked by levels, such as where it states '<player name>
(skill <skill>)' as an action menu text).

'''Location updating'''

After the appearance updating, the client starts to update that player's
location relative to our player. The player is added to the local player
list and it's last update cycle time. It then reads a 1 bit quantity
that defines whether or not the client has a chunk in the player update
block list. If it does, it adds it to the updating list. The next bit
states whether or not to discard the awaiting-waypoint queue, such as
when teleporting. It then reads to 5 bit values that determine the
players relative X and Y coordinates to our player. The local player
area is 16x16, so if the delta of the two coordinates is \> 15, 32 is
subtracted from it to signify the player is on the other side of ours.
The client then sets the player's position, ending the player list
updating process.

'''Update block flag-based updating'''

The following is what most people think of when they say 'update mask'
and 'update flag'. This process of the updating procedure is very
important. It begins with looping through ALL players in the local
player update list, reading an unsigned byte which from now on will be
called the update flag. All further updates are seen to be 'included' by
comparing a bitwise mask to this flag. If the flag has the bits for 0x40
all on, this signifies that the flag was too large for a simple unsigned
byte and reads in another unsigned byte, which it uses as the upper
unsigned byte, therefore the update flag is an unsigned little-endian
short. The client then passes off the data to a helper method which
processes all updates this flag signifies.

Inside this method, many different bitwise masks are compared to the
player's flag, and if the mask is set, logic is performed. These masks
are frequently called update masks. A list of player update masks are
below:

-   '''0x400''' The 0x400 mask is used to update the player so they
    appear to be asynchronously animating and walking. This mask is
    often used for the \[http://runescape.wikia.com/wiki/Agility
    Agility\] skill. The data associated goes in order of: byte (type C)
    which is the first location's X coordinate value, byte (type S)
    which is the first location's Y coordinate value, byte (type S)
    which is the second location's X coordinate value, byte (type C) the
    second location's Y coordinate value. After the locations are
    written, there is a required movement speed which is written as a
    short which marks how fast to move from position 1 to position 2.
    Another short (type A) is written as the movement speed going from
    position 2 to position 1. Finally one byte is written to end the
    mask block, which marks the direction.

-   '''0x100''' The 0x100 mask is responsible for player graphics
    updating. The data associated is a little-endian unsigned short
    which represents the graphics id, and an int which is the graphics
    delay.

-   '''0x8''' Animations are handled by the 0x8 mask. The payload for
    this update is a little-endian unsigned short that is the animation
    id, and an unsigned inversed byte (Special C) which states the
    animation's delay.

-   '''0x4''' The beloved 0x4 mask takes care of forced player text that
    is only displayed above the player's model. The only data associated
    with this is a jagex ASCII string with a terminator of 10.

-   '''0x80''' Unlike the previous, the 0x80 mask handles normal player
    chat text. The client will read a little-endian unsigned short which
    holds chat text attributes. It holds the text color and chat
    effects. Next, the client reads an unsigned byte which states the
    player's priveleges (normal player, player moderator, moderator,
    staff) to give the chatter's name a crown. Right behind it trails an
    unsigned inversed byte that gives chat text length in bytes.
    Trailing afterwards is dictionary-compressed chat text. All chat
    text characters become indexes into a valid character table and are
    written as nibbles (4 bit quantities).

-   '''0x1''' Updating the player's current interacting-entity is done
    via mask 0x1. The entity id is written as a little-endian unsigned
    short.

-   '''0x10''' The 0x10 mask updates appearance of the player in exact
    same way as in updating player list. Only difference is that
    appearance is updated from a set-sized buffer filled from the
    current buffer. An unsigned inversed byte is read first which
    describes appearance buffer size, and the buffer is filled.

-   '''0x2''' Facing coordinate updating is signified by the 0x2 mask.
    The player's facing-towards X and Y are set to read values;
    specifically, an unsigned lower-inverted short and little-endian
    unsigned short, respectively.

-   '''0x20''' Notifying client's of a player's health is done via the
    0x20 mask. The hitpoint damage done to the player is sent as an
    unsigned byte, followed by the hit type as a positive inverted byte.
    The player's current and max health are read as an unsigned inverted
    byte and unsigned byte, respectively.

-   '''0x200''' The 0x200 mask acts in the same way as the 0x20 mask and
    is most likely associated with special attacks from weapons that
    have the ability to hit twice at the same time. Hitpoint damage is
    an unsigned byte, the hit type an unsigned inverted byte, and the
    current and maximum health being an unsigned byte and unsigned
    inverted byte, respectively.

After the client processes every single player in the update player
list, it ends player updating.

==Game Protocol== The game protocol is the in-game communication of
player actions between the server and client. <br/> ===Server -\> Client
Packets===

  {
  ---

! Opcode ! Type ! Length (bytes) ! Name ! Description \|- \| 1 \| FIXED
\| 0 \| \[\[317 Animation reset\|Animation reset\]\] \| Resets all
animations in the immediate area. \|- \| 4 \| FIXED \| 6 \| \[\[317 Draw
graphic at position\|Draw graphic at position\]\] \| Draw a graphic at a
given x/y position after a delay. \|- \| 8 \| FIXED \| 4 \| \[\[317 Set
interface model\|Set interface model\]\] \| Draw a given model on a
given interface. \|- \| 24 \| FIXED \| 1 \| \[\[317 Flash sidebar\|Flash
sidebar\]\] \| Causes a sidebar icon to start flashing. \|- \| 27 \|
FIXED \| 0 \| \[\[317 Input amount\|Input amount\]\] \| Displays the
"Input amount" interface. \|- \| 34 \| VARIABLE\_SHORT \| N/A \| \[\[317
Draw items on interface\|Draw items on interface\]\] \| Draw a
collection of items on an interface. \|- \| 35 \| FIXED \| 4 \| \[\[317
Camera shake\|Camera shake\]\] \| Causes the camera to shake. \|- \| 36
\| FIXED \| 3 \| \[\[317 Force client setting\|Force client setting\]\]
\| Forcefully changes a client's setting's value. Also changes the
default value for that setting. \|- \| 44 \| FIXED \| 5 \| \[\[317 Send
ground item\|Send ground item\]\] \| Place an item stack on the ground.
\|- \| 50 \| FIXED \| 9 \| \[\[317 Send add friend\|Send add friend\]\]
\| Sends a friend to the friend list. \|- \| 53 \| VARIABLE\_SHORT \|
N/A \| \[\[317 Draw items on interface\|Draw items on interface\]\] \|
Draw a collection of items on an interface. \|- \| 60 \| VARIABLE\_SHORT
\| N/A \| \[\[317 Begin processing position related packets\|Begin
processing position related packets\]\] \| Begin processing position
related packets. \|- \| 61 \| FIXED \| 1 \| \[\[317 Show
multi-combat\|Show multi-combat\]\] \| Shows the player that they are in
a multi-combat zone. \|- \| 64 \| FIXED \| 2 \| \[\[317 Delete ground
item\|Delete ground item\]\] \| Delete ground item. \|- \| 65 \|
VARIABLE\_SHORT \| N/A \| \[\[317 NPC updating\|NPC updating\]\] \| NPC
updating \|- \| 68 \| FIXED \| 0 \| \[\[317 Reset button state\|Reset
button state\]\] \| Resets the button state for all buttons. \|- \| 70
\| FIXED \| 6 \| \[\[317 Interface offset\|Interface offset\]\] \| Sets
the offset for drawing of an interface. \|- \| 71 \| FIXED \| 3 \|
\[\[317 Send sidebar interface\|Send sidebar interface\]\] \| Assigns an
interface to one of the tabs in the game sidebar. \|- \| 72 \| FIXED \|
2 \| \[\[317 Clear inventory\|Clear inventory\]\] \| Clears an
interface's inventory. \|- \| 73 \| FIXED \| 4 \| \[\[317 Load map
region\|Load map region\]\] \| Loads a new map region. \|- \| 74 \|
FIXED \| 4 \| \[\[317 Play song\|Play song\]\] \| Starts playing a song.
\|- \| 75 \| FIXED \| 4 \| \[\[317 NPC head on interface\|NPC head on
interface\]\] \| Place the head of an NPC on an interface \|- \| 78 \|
FIXED \| 0 \| \[\[317 Reset destination\|Reset destination\]\] \| Resets
the players' destination. \|- \| 79 \| FIXED \| 4 \| \[\[317 Scroll
position\|Scroll position\]\] \| Sets the scrollbar position of an
interface. \|- \| 81 \| VARIABLE \| N/A \| \[\[317 Begin player
updating\|Begin player updating\]\] \| Begins the player update
procedure \|- \| 84 \| FIXED \| 7 \| \[\[317 Edit ground item
amount\|Edit ground item amount\]\] \| Edit ground item amount \|- \| 85
\| FIXED \| 2 \| \[\[317 Set local player coordinates\|Set local player
coordinates\]\] \| Set local player coordinates \|- \| 87 \| FIXED \| 7
\| \[\[317 Force client setting\|Force client setting\]\] \| Forcefully
changes a client's setting's value. Also changes the default value for
that setting. \|- \| 97 \| FIXED \| 2 \| \[\[317 Show interface\|Show
interface\]\] \| Displays a normal interface. \|- \| 99 \| FIXED \| 1 \|
\[\[317 Minimap State\|Minimap State\]\] \| Sets the mini map's state.
\|- \| 101 \| FIXED \| 3 \| \[\[317 Object removal\|Object removal\]\]
\| Sends an object removal request to the client. \|- \| 104 \| VARIABLE
\| N/A \| \[\[317 Player Option\|Player Option\]\] \| Adds a player
option to the right click menu of player(s). \|- \| 105 \| FIXED \| 4 \|
\[\[317 Play sound in location\|Play sound in location\]\] \| Play sound
in location. \|- \| 106 \| FIXED \| 1 \| \[\[317 Interface over
tab\|Interface over tab\]\] \| Draws an interface over the tab area. \|-
\| 107 \| FIXED \| 0 \| \[\[317 Reset camera\|Reset camera\]\] \| Resets
the camera position. \|- \| 109 \| FIXED \| 0 \| \[\[317
Logout\|Logout\]\] \| Disconnects the client from the server. \|- \| 110
\| FIXED \| 1 \| \[\[317 Run energy\|Run energy\]\] \| Sends the players
run energy level. \|- \| 114 \| FIXED \| 2 \| \[\[317 System
update\|System update\]\] \| Sends how many seconds until a 'System
Update.' \|- \| 117 \| N/A \| N/A \| \[\[317 Create Projectile\|Create
Projectile\]\] \| Creates a projectile. \|- \| 121 \| FIXED \| 4 \|
\[\[317 Song Queue\|Song Queue\]\] \| Queues a song to be played next.
\|- \| 122 \| FIXED \| 4 \| \[\[317 Interface color\|Interface color\]\]
\| Changes the color of an interface. \|- \| 126 \| VARIABLE\_SHORT \|
N/A \| \[\[317 Set interface text\|Set interface text\]\] \| Attaches
text to an interface. \|- \| 134 \| FIXED \| 6 \| \[\[317 Skill
level\|Skill level\]\] \| Sends a skill level to the client. \|- \| 142
\| FIXED \| 2 \| \[\[317 Show inventory interface\|Show inventory
interface\]\] \| Show inventory interface \|- \| 147 \| FIXED \| 14 \|
\[\[317 Player to object transformation\|Player to object
transformation\]\] \| Player to object transformation \|- \| 151 \|
FIXED \| 5 \| \[\[317 Object spawn\|Object spawn\]\] \| Sends an object
spawn request to the client. \|- \| 156 \| FIXED \| 3 \| \[\[317 Remove
non-specified ground items???\|Remove non-specified ground items???\]\]
\| Remove non-specified ground items?????? \|- \| 160 \| FIXED \| 4 \|
\[\[317 Spawn an animated object???\|Spawn an animated object???\]\] \|
Shows an interface in the chat box?????? \|- \| 164 \| FIXED \| 2 \|
\[\[317 Chat interface\|Chat interface\]\] \| Shows an interface in the
chat box. \|- \| 166 \| FIXED \| 6 \| \[\[317 Spin camera\|Spin
camera\]\] \| Spin camera \|- \| 171 \| FIXED \| 3 \| \[\[317 Hidden
Interface\|Hidden Interface\]\] \| Sets an interface to be hidden until
hovered over. \|- \| 174 \| FIXED \| N/A \| \[\[317 Audio\|Audio\]\] \|
Sets what audio/sound is to play at a certain time. \|- \| 176 \| FIXED
\| 10 \| \[\[317 Open welcome screen\|Open welcome screen\]\] \|
Displays the welcome screen. \|- \| 177 \| FIXED \| 6 \| \[\[317 Set
cutscene camera\|Set cutscene camera\]\] \| Set cutscene camera \|- \|
185 \| FIXED \| 2 \| \[\[317 Player head to interface\|Player head to
interface\]\] \| Sends the players head model to an interface \|- \| 187
\| FIXED \| 0 \| \[\[317 Enter name\|Enter name\]\] \| Displays the
"Enter name" interface. \|- \| 196 \| VARIABLE\_BYTE \| N/A \| \[\[317
Send private message\|Send private message\]\] \| Sends a private
message to another player. \|- \| 200 \| FIXED \| 4 \| \[\[317 Interface
animation\|Interface animation\]\] \| Sets an interface's model
animation. \|- \| 206 \| FIXED \| 3 \| \[\[317 Chat settings\|Chat
settings\]\] \| Sends the chat privacy settings. \|- \| 208 \| FIXED \|
2 \| \[\[317 Walkable interface\|Walkable interface\]\] \| Displays an
interface in walkable mode. \|- \| 214 \| VARIABLE\_SHORT \| N/A \|
\[\[317 Send add ignore\|Send add ignore\]\] \| Sends a ignored player
to the ignore list. \|- \| 215 \| FIXED \| 7 \| \[\[317 Spawn ground
item for all except specified player\|Spawn ground item for all except
specified player\]\] \| Spawn ground item for all except specified
player \|- \| 218 \| FIXED \| 2 \| \[\[317 Open chatbox interface\|Open
chatbox interface\]\] \| Opens an interface over the chatbox. \|- \| 219
\| FIXED \| 0 \| \[\[317 Clear screen\|Clear screen\]\] \| Clears the
screen of all open interfaces. \|- \| 221 \| FIXED \| 1 \| \[\[317
Friends list status\|Friends list status\]\] \| Friends list load
status. \|- \| 230 \| FIXED \| 8 \| \[\[317 Interface model
rotation\|Interface model rotation\]\] \| Sets an interface's model
rotation and zoom \|- \| 240 \| FIXED \| 2 \| \[\[317 Weight\|Weight\]\]
\| Sends the players weight amount. \|- \| 241 \| VARIABLE\_SHORT \| N/A
\| \[\[317 Construct map region\|Construct map region\]\] \| Constructs
a dynamic map region using a palette of 8\*8 tiles. \|- \| 246 \| FIXED
\| 6 \| \[\[317 Interface item\|Interface item\]\] \| Displays an item
model inside an interface. \|- \| 248 \| FIXED \| 4 \| \[\[317 Inventory
overlay\|Inventory overlay\]\] \| Displays an interface over the sidebar
area. \|- \| 249 \| FIXED \| 3 \| \[\[317 Initialize player\|Initialize
player\]\] \| Sends the player's membership status and their current
index on the server's player list. \|- \| 253 \| VARIABLE\_BYTE \| N/A
\| \[\[317 Send message\|Send message\]\] \| Sends a server message
(e.g. 'Welcome to RuneScape') or trade/duel request. \|- \| 254 \|
VARIABLE\_BYTE \| N/A \| \[\[317 Display hint icon\|Display hint
icon\]\] \| Displays a hint icon. \|- \|}

===Client -\> Server Packets===

  {
  ---

! Opcode ! Type ! Length (bytes) ! Name ! Description \|- \| 0 \| FIXED
\| 0 \| \[\[317 Idle\|Idle\]\] \| Sent when there are no actions being
performed by the player for this cycle. \|- \| 3 \| FIXED \| 1 \|
\[\[317 Focus change\|Focus change\]\] \| Sent when the game client
window goes out of focus. \|- \| 4 \| VARIABLE BYTE \| N/A \| \[\[317
Chat\|Chat\]\] \| Sent when the player enters a chat message. \|- \| 14
\| FIXED \| 8 \| \[\[317 Item on player\|Item on player\]\] \| Sent when
a player uses an item on another player. \|- \| 16 \| FIXED \| 1 \|
\[\[317 Alternate item option 2\|Alternate item option 2\]\] \| Sent
when a player uses an item. This is an alternate item option. \|- \| 17
\| FIXED \| 2 \| \[\[317 NPC action 2\|NPC action 2\]\] \| Sent when a
player clicks the second option of an NPC. \|- \| 18 \| FIXED \| 2 \|
\[\[317 NPC action 4\|NPC action 4\]\] \| Sent when a player clicks the
fourth option of an NPC. \|- \| 21 \| FIXED \| 2 \| \[\[317 NPC action
3\|NPC action 3\]\] \| Sent when a player clicks the third option of an
NPC. \|- \| 25 \| FIXED \| 10 \| \[\[317 Item on floor\|Item on
floor\]\] \| Sent when a player uses an item on another item thats on
the floor. \|- \| 39 \| FIXED \| 2 \| \[\[317 Trade answer\|Trade
answer\]\] \| Sent when a player answers a trade request from another
player. \|- \| 40 \| FIXED \| N/A \| \[\[317 NPC
Dialogue\|NpcDialogue\]\] \| Sent when a player clicks the "Click here
to continue" on any dialogue. \|- \| 41 \| FIXED \| 6 \| \[\[317 Equip
item\|Equip item\]\] \| Sent when a player equips an item. \|- \| 43 \|
FIXED \| 6 \| \[\[317 Bank 10 items\|Bank 10 items\]\] \| Sent when a
player banks 10 of a certain item. \|- \| 53 \| FIXED \| 4 \| \[\[317
Item on item\|Item on item\]\] \| Sent when a player uses an item with
another item. \|- \| 70 \| FIXED \| 6 \| \[\[317 Object action 3\|Object
action 3\]\] \| Sent when the player clicks the third action available
for an object. \|- \| 72 \| FIXED \| 2 \| \[\[317 Attack (NPC)\|Attack
(NPC)\]\] \| Sent when a player attacks an NPC. \|- \| 73 \| FIXED \| 2
\| \[\[317 Trade request\|Trade request\]\] \| Sent when a player
requests a trade with another player. \|- \| 74 \| FIXED \| 8 \| \[\[317
Remove ignore\|Remove ignore\]\] \| Sent when a player removes a player
from their ignore list. \|- \| 79 \| FIXED \| 6 \| \[\[317 Light
item\|Light item\]\] \| Sent when a player attempts to light logs on
fire. \|- \| 86 \| FIXED \| 4 \| \[\[317 Camera movement\|Camera
movement\]\] \| Sent when the player moves the camera. \|- \| 87 \|
FIXED \| 6 \| \[\[317 Drop item\|Drop item\]\] \| Sent when a player
wants to drop an item onto the ground. \|- \| 95 \| FIXED \| 3 \|
\[\[317 Privacy options\|Privacy options\]\] \| Sent when a player
changes their privacy options (i.e. public chat). \|- \| 98 \|
VARIABLE\_BYTE \| N/A \| \[\[317 Walk on command\|Walk on command\]\] \|
Sent when the player should walk somewhere according to a certain action
performed, such as clicking an object. \|- \| 101 \| FIXED \| 13 \|
\[\[317 Design screen\|Design screen\]\] \| Sent when a player is
choosing their character design options. \|- \| 103 \| VARIABLE\_BYTE \|
N/A \| \[\[317 Player command\|Player command\]\] \| Sent when the
player enters a command in the chat box (e.g. "::command") \|- \| 117 \|
FIXED \| 6 \| \[\[317 Bank 5 items\|Bank 5 items\]\] \| Sent when a
player banks 5 of a certain item. \|- \| 121 \| FIXED \| 0 \| \[\[317
Loading finished\|Loading finished\]\] \| Sent when the client finishes
loading a map region. \|- \| 122 \| FIXED \| 6 \| \[\[317 Item action
1\|Item action 1\]\] \| Sent when the player clicks the first option of
an item, such as "Bury" for bones. \|- \| 126 \| VARIABLE BYTE \| N/A \|
\[\[317 Private message\|Private message\]\] \| Sent when a player sends
a private message to another player. \|- \| 129 \| FIXED \| 6 \| \[\[317
Bank all items\|Bank all items\]\] \| Sent when a player banks all of a
certain item that they have in their inventory. \|- \| 130 \| FIXED \| 0
\| \[\[317 Close window\|Close window\]\] \| Sent when a player presses
the close, exit or cancel button on an interface. \|- \| 131 \| FIXED \|
4 \| \[\[317 Mage NPC\|Mage NPC\]\] \| Sent when a player uses magic
attacks on an NPC. \|- \| 132 \| FIXED \| 6 \| \[\[317 Object action
1\|Object action 1\]\] \| Sent when the player clicks the first option
of an object, such as "Cut" for trees. \|- \| 133 \| FIXED \| 8 \|
\[\[317 Add ignore\|Add ignore\]\] \| Sent when a player adds a player
to their ignore list. \|- \| 135 \| FIXED \| 6 \| \[\[317 Bank X items
part-1\|Bank X items part-1\]\] \| Sent when a player requests to bank
an X amount of items. \|- \| 139 \| FIXED \| 2 \| \[\[317
Follow\|Follow\]\] \| Sent when a player clicks the follow option on
another player. \|- \| 145 \| FIXED \| 6 \| \[\[317 Unequip
item\|Unequip item\]\] \| Sent when a player unequips an item. \|- \|
155 \| FIXED \| 2 \| \[\[317 NPC action 1\|NPC action 1\]\] \| Sent when
a player clicks first option of an NPC, such as "Talk." \|- \| 164 \|
VARIABLE\_BYTE \| N/A \| \[\[317 Regular walk\|Regular walk\]\] \| Sent
when the player walks regularly. \|- \| 185 \| FIXED \| 2 \| \[\[317
Button click\|Button click\]\] \| Sent when a player clicks an in-game
button. \|- \| 188 \| FIXED \| 8 \| \[\[317 Add friend\|Add friend\]\]
\| Sent when a player adds a friend to their friend list. \|- \| 192 \|
FIXED \| 12 \| \[\[317 Item on object\|Item on object\]\] \| Sent when a
a player uses an item on an object. \|- \| 202 \| FIXED \| 0 \| \[\[317
Idle logout\|Idle logout\]\] \| Sent when the player has become idle and
should be logged out. \|- \| 208 \| FIXED \| 4 \| \[\[317 Bank X items
part-2\|Bank X items part-2\]\] \| Sent when a player enters an X amount
of items they want to bank. \|- \| 210 \| FIXED \| 0 \| \[\[317 Region
change\|Region change\]\] \| Sent when a player enters a new map region.
\|- \| 214 \| FIXED \| 7 \| \[\[317 Move item\|Move item\]\] \| Sent
when a player moves an item from one slot to another. \|- \| 215 \|
FIXED \| 8 \| \[\[317 Remove friend\|Remove friend\]\] \| Sent when a
player removes a friend from their friend list. \|- \| 218 \| FIXED \| 8
\| \[\[317 Report player\|Report player\]\] \| Sent when a player
reports another player. \|- \| 236 \| FIXED \| 6 \| \[\[317 Pickup
ground item\|Pickup ground item\]\] \| Sent when the player picks up an
item from the ground. \|- \| 237 \| FIXED \| 8 \| \[\[317 Magic on
items\|Magic on items\]\] \| Sent when a player casts magic on the items
in their inventory. \|- \| 241 \| FIXED \| 4 \| \[\[317 Mouse
click\|Mouse click\]\] \| Sent when the player clicks somewhere on the
game screen. \|- \| 248 \| VARIABLE\_BYTE \| N/A \| \[\[317 Map
walk\|Map walk\]\] \| Sent when the player walks using the map. Has 14
additional (assumed to be anticheat) bytes added to the end of it that
are ignored. \|- \| 249 \| FIXED \| 4 \| \[\[317 Magic on player\|Magic
on player\]\] \| Sent when a player attempts to cast magic on another
player. \|- \| 252 \| FIXED \| 6 \| \[\[317 Object action 2\|Object
action 2\]\] \| Sent when the player clicks the second option available
for an object. \|- \| 253 \| FIXED \| 6 \| \[\[317 Ground Item
Action\|Ground Item Action\]\] \| Sent when the player clicks the first
option for a ground item (I.E. 'Light Logs') \|- \|}
