\[\[Category Packet\]\] \[\[Category Packet 317\]\] {{packet\|name=Item
on floor\|description=Sent when a player uses an item on another item
thats on the floor.\|opcode=25\|type=Fixed\|length=10\|revision=317}} ==
Item on Floor==

=== Description ===

This packet is sent when a player uses an item on another item thats on
the floor.

=== Packet Structure === {\|border=2 ! Data Type ! Description \|- \|
\[\[Data Types\#Little Endian\|Little Endian\]\] \[\[Data
Types\#Standard data types\|Short\]\] \| The interface ID. \|- \|
Unsigned \[\[Data Types\#Standard data types\|Short\]\] \[\[Data
Types\#Non Standard Data Types\|Special A\]\] \| The item being used ID.
\|- \| \[\[Data Types\#Standard data types\|Short\]\] \| The floor items
ID. \|- \| Unsigned \[\[Data Types\#Standard data types\|Short\]\]
\[\[Data Types\#Non Standard Data Types\|Special A\]\] \| The Y
coordinate of the item. \|- \| Unsigned \[\[Data Types\#Little
Endian\|Little Endian\]\] \[\[Data Types\#Standard data types\|Short\]\]
\[\[Data Types\#Non Standard Data Types\|Special A\]\] \| The items slot
ID. \|- \| \[\[Data Types\#Standard data types\|Short\]\] \| The X
coordinate of the item. \|- \|}
