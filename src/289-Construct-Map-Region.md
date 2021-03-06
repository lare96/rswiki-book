{{packet\|name=Construct Map Region\|description=Constructs the map
region\|opcode=241\|type=Variable Short\|length=N/A\|revision=317}} ==
Construct Map Region ==

=== Description ===

This packet constructs the map region.

=== Packet Structure === {\|border=2 ! Data Type ! Description \|- \|
\[\[Data Types\#Standard data types\|Additional Short\]\] \| The players
region y plus 6. \|- \| \[\[Data Types\#Standard data types\|Short\]\]
\| The players region x plus 6. \|- \|}

=== Other Information ===

After the region y is sent, you need to initialize the bit access. Then,
loop through the z (which can only go up to 3). Still in the for-loop,
you need to go through the x's (up to 12). Then, loop through the y's
(up to 12).

All of this is in the all three for-loops!

Step 1: Then you'll get the tile of x, y, and z.

Step 2: Then you need to send the bits 1 and (if tile is null) 1
otherwise, 0.

Step 3: Check if the tile is not null. Within this if-statement, you put
these bits...

{\|border=2 ! Method \|- \| putBits(26, tile.getX() \<\< 14 \|
tile.getY() \<\< 3 \| tile.getZ() \<\< 24 \| tile.getRotation() \<\< 1)
\|- \|}

Out of the for-loops!

Step 1: Finish the bit access and send the region x.

Done.
