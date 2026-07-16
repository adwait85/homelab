# day 6 — actually doing it this time

so with the new plan in hand, went for it for real this time. ont stays on native
pppoe forever, its lan side just becomes the transit link into opnsense instead of
trying to replace it with anything.

logged into the ont with admin account this time, not user one which is usally found on the back sid of ont, since that's the one that
actually lets you touch the lan ip and static routing pages. changed the lan ip away
from the normal home range over to the transit range, killed the ont's own dhcp server
completely since opnsense is meant to be the only thing handing out addresses on this
network now, and added two static routes back to the trusted lan and camera subnets so
reply traffic would actually find its way home instead of just vanishing outbound.

the second i saved that lan ip change, i got booted out of the ont's admin page
immediately. expected, that's just what happens when you change the address you're
currently connected through. also meant the whole house lost internet right then, since
nothing was cabled into the new setup yet. so this was the actual point of no return for
the night.

wired everything up — server, switch, all of it — and instead of running a cable to the
r480t+ like the original plan called for, ran it straight into the ont's lan port
instead. fired up the opnsense vm, watched it boot on the console, pinged out to the ont
and then out to 8.8.8.8 from opnsense's own shell. both came back. internet's alive
again, routed through opnsense this time instead of straight through the ont like
before.

tested from a normal device next — released and renewed the ip, got handed the right
gateway, browser loaded a page. felt like it worked.

except three things had gone completely dark. casaos, matrix, home assistant — all
unreachable, despite every one of them having perfectly correct ip addresses and
gateways set. took a bit to figure out why. turned out to be leftover fallout from
something i'd changed earlier without fully thinking through the consequence — i'd
repurposed the server's onboard nic into a direct link to the thin client, which meant
the network bridge that all three of those services were still plugged into was now
connected to nothing. it wasn't broken exactly, it just didn't lead anywhere anymore.

fix was moving all three onto the actual working network bridge and correctly tagging
them for the right vlan, since that bridge carries multiple vlans at once and doesn't
know which one any given thing belongs to unless you tell it explicitly. did that for
casaos, same fix for matrix. home assistant needed one extra step on top since it's a
full vm running its own os — fixing the bridge setting on the outside only fixes the
outer path, the gateway and dns settings inside home assistant itself needed a separate
manual fix too.

once those three were back, moved on to dns filtering. wanted every device on the
network resolving through adguard instead of straight to public dns. set up forwarding
in opnsense. looked right, saved fine. internet still worked by raw ip, but nothing
resolved by name anymore. spent way too long on this before realizing i'd put the
forwarding address into completely the wrong settings screen — two fields in opnsense
that look almost identical but do genuinely different things, and i'd been configuring
the wrong one this whole time. moved it to the correct one and it started working
immediately.

by the end of the night: internet flowing through opnsense properly, all core services
back up, dns filtering actually filtering. genuinely felt like the network was mine now,
not just something i'd copied instructions onto.

cameras were still sitting on the old setup at this point though — that's its own story,
next time.
