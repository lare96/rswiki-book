\[\[Category Packet\]\] \[\[Category Packet 317\]\] == Object spawn ==

=== Description ===

This packet requests the client to spawn an object.

=== Packet Structure === {\|border=2 ! Data Type ! Description \|- \|
subtrahend \[\[Data Types\#byte\|byte\]\] \| 0 \|- \| Little endian
\[\[Data Types\#byte\|byte\]\] \| ObjectID \|- \| subtrahend \[\[Data
Types\#byte\|byte\]\] \| object type \<\< 2 + object rotation & 3 \|-
\|}
