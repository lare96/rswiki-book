\[\[Category Packet\]\] \[\[Category Packet 317\]\] {{packet\|name=Bank
all items\|description=Sent when a player banks all of a certain item
they have in their
inventory.\|opcode=129\|type=Fixed\|length=6\|revision=317}} == Bank 10
Items ==

=== Description ===

This packet is sent when a player banks all of a certain item they have
in their inventory. <br> '''Note:''' This packet is also used for
selling/buying 10 items at a shop.

=== Packet Structure === {\|border=2 ! Data Type ! Description \|- \|
Unsigned \[\[Data Types\#Standard data types\|Short\]\] \[\[Data
Types\#Non Standard Data Types\|Special A\]\] \| The items slot ID. \|-
\| Unsigned \[\[Data Types\#Standard data types\|Short\]\] \| The
interface ID. \|- \| Unsigned \[\[Data Types\#Standard data
types\|Short\]\] \[\[Data Types\#Non Standard Data Types\|Special A\]\]
\| The item ID. \|- \|}
