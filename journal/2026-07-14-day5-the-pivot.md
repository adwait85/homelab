# day 5 — figuring out the actual fix

came back to this a bit annoyed after the failed night, but figured before planning
anything new i should actually confirm exactly what state everything was in, rather than
assume. so went through each device one by one.

ont — back to completely normal, factory pppoe mode, like nothing ever happened.
r480t+ — fully unplugged, but still had the kerala vision pppoe username saved on it
from the failed attempt, sitting there unused. proxmox — untouched, all the opnsense
config i'd built earlier was still sitting there exactly as configured, just waiting for
a working wan path to actually use it.

while poking around the ont again i found something i'd missed before — it actually has
two separate admin logins. the basic one, epadmin, which is what i'd been using the
whole time. and a second one, telecomadmin, with way more access. logged in with that
one out of curiosity and suddenly could see the full wan profile screen properly —
connection name, vlan id, and sitting right there in plain text, the actual pppoe
username. which meant the credentials were never the problem. the ont had been
authenticating fine this entire time, on its own, using its own trusted identity.

and that's what actually flipped the plan for me. i'd been so focused on getting the
r480t+ to take over pppoe that i didn't stop and ask why. the ont already does it
perfectly, every single day, without me touching anything. so why not just... let it keep
doing that forever, and build the rest of the network around it instead of trying to
replace it.

new plan: ont stays in native pppoe mode permanently. instead of bridging it, just
change its lan side away from the normal home ip range and repurpose that as the
transit link straight into opnsense's wan port. opnsense doesn't even need to know
anything changed — no double nat, no bridge mode, no fighting kerala vision's mac
binding ever again, because the device authenticating never changes.

so that's the new plan — leave the ont alone, let it keep doing pppoe like it already
does, and just change its lan side to become the transit link straight into opnsense's
wan port instead. opnsense itself isn't even set up yet at this point, that's the actual
next step — but at least now there's a clear plan for what its wan side needs to look
like when i do get to it.

r480t+ is out of the main plan for now. not getting rid of it though — thinking about
giving it a smaller, separate job later, something completely away from the main network
path so it can't cause the same kind of trouble again.

honestly this feels like the right way to solve it. instead of forcing the old plan to
work, actually figure out why it broke and build around that instead.
