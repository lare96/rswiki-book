\[\[Category Packet\]\] \[\[Category Packet 317\]\] {{packet\|name=Equip
item\|description=Sent when a player equips an
item.\|opcode=41\|type=Fixed\|length=6\|revision=317}} == Equip Item ==

=== Description ===

This is sent when a player equips an item in-game.

=== Packet Structure ===

{\| border=2 ! Data type ! Description \|- \| Unsigned \[\[Data
Types\#Standard data types\|Short\]\] \| The ID of the item. \|- \|
Unsigned \[\[Data Types\#Standard data types\|Short\]\] \[\[Data
Types\#Non Standard Data Types\|Special A\]\] \| The slot of the item.
\|- \| Unsigned \[\[Data Types\#Standard data types\|Short\]\] \[\[Data
Types\#Non Standard Data Types\|Special A\]\] \| The ID of the
interface. \|- \|}
