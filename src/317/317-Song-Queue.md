\[\[Category Packet\]\] \[\[Category Packet 317\]\] {\|
style="float:right; margin:1em; width:250px;" cellpadding="3"
cellspacing="0" border="1" \|- \| colspan="2"
style="background-color:\#3c5;" \| '''Song Queue'''<br/>Queue's a song
to be played next. \|- !'''Opcode''' \| 121 \|- ! '''Type''' \| FIXED
\|- ! '''Length''' \| 4 \|- \|} == Song Queue ==

=== Description ===

This packet queue's a song to be played next. The client then proceeds
to request the queued song using the ondemand protocol.

=== Packet Structure === {\|border=2 ! Data Type ! Description \|- \|
\[\[Data Types\#Little Endian\|Little Endian\]\] \[\[Data
Types\#Standard data types\|Short\]\] \[\[Data Types\#Non Standard Data
Types\|Special A\]\] \| The id of the next song. \|- \| \[\[Data
Types\#Standard data types\|Short\]\] \[\[Data Types\#Non Standard Data
Types\|Special A\]\] \| The id of the previous song. \|- \|}
\[\[Category Packet\]\] \[\[Category Packet 317\]\]
