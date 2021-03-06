\[\[Category Packet\]\] \[\[Category Packet 317\]\] {{packet\|name=Send
ignored users\|description=Sends a list of all the ignored player
IDs\|opcode=214\|type=VARIABLE\_SHORT\|length=N/A\|revision=317}} ==
Send ignored users ==

=== Description ===

Sends the IDs of all the users that this player has in their ignore.

This packet has a slightly different structure than the other packets.

int entries = packetSize / 8; for (int i = 0; i \< entries; i++) {
ignoreList\[i\] = stream.readLong(); }

By looking at the rest of the 317 protocol, there doesn't seem to be a
way to change the list dynamically. It seems as though that whenever the
player decides to add or remove a player from their list, it must send
all the values again.

=== Packet Structure === {\|border=2 ! Data Type ! Description \|- \|
\[\[Data Types\#Standard Data Types\|Long\]\] \| The Unique Identifier
of the player(s) (possibly determined by their username). \|- \|}
