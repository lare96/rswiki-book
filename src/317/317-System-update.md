\[\[Category Packet\]\] \[\[Category Packet 317\]\]
{{packet\|name=System update\|description=Sends how many seconds until a
'System Update.'\|opcode=114\|type=Fixed\|length=2\|revision=317}} ==
System Update ==

=== Description ===

A timer showing how many seconds until a 'System Update' will appear in
the lower left hand corner of the game screen. After the timer reaches 0
all players are disconnected and are unable to log in again until server
is restarted. Players connecting will receive a message stating, "The
server is being updated. Please wait 1 minute and try again." (unless
stated otherwise).

=== Packet Structure ===

{\| border=2 ! Data type ! Description \|- \| \[\[Data Types\#Little
Endian\|Little Endian\]\] \[\[Data Types\#Standard data types\|Short\]\]
\| Time until an update. \|- \|}
