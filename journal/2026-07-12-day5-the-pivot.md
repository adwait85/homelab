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

went back through the opnsense config i'd already built weeks earlier just to check how
much would need to change for this. turned out to be almost nothing — the wan side was
already sitting on a static address in the exact transit range this new plan needed. all
that groundwork wasn't wasted after all, it was just waiting for the right wan-side
device to plug into it.

r480t+ officially got benched from the main plan at this point. not thrown out though —
started thinking about giving it a smaller, lower-stakes job later on, something
completely separate from the main network path where it couldn't cause the same kind of
trouble again.

felt like the actual turning point of the whole project, not gonna lie. going from
"trying to force a workaround to work" to "actually understanding why it broke and
designing around that" is a different kind of problem solving, and it's the version that
actually held up long term.
