# day 4 — the first attempt, and the first real wall

alright so by this point hardware side was sorted, both proxmox boxes were up, had the
switch, had the r480t+ sitting there with an actual purpose now instead of just being an
impulse buy. time to actually build the thing.

got the switch vlans configured first — split things into transit, trusted lan, cameras,
and a separate one just for pfsync between the two opnsense boxes down the line. set up
the proxmox bridges to match, one plain one for the wan side, one proper vlan-aware trunk
for everything else. all of this went fine, no drama, just methodical setup work.

the actual plan for going live was: put the huawei ont into bridge mode, so it stops
doing pppoe itself and just passes the raw session straight through. then have the
r480t+ pick up that pppoe session directly, authenticate with kerala vision, and sit as
the transit device in front of opnsense. clean plan on paper. had literally everything
else already built and waiting — opnsense fully configured, carp addresses set, dhcp
ranges done, firewall rules done. this was supposed to be the easy part.

flipped the ont into bridge mode. plugged in the pppoe username and password on the
r480t+. and it just sat there. "connecting..." forever. never came up.

tried the obvious fix — cloned the mac address off the sticker on the back of the ont
onto the r480t+'s wan port. still nothing. dug into kerala vision's own customer portal,
found what looked like the wan mac listed on my account page there too, tried that one
as well. still nothing.

spent a good while on this before accepting what was probably going on — kerala vision
almost certainly binds the pppoe session to whichever device last successfully
authenticated, which at this point was permanently the ont itself. swapping in a
different physical device, even with a cloned mac, apparently isn't enough to fool
whatever's happening on their end. would probably need an actual call to their support
line to get that session released, and at that point, on a live migration night, that's
not a rabbit hole i wanted to go down.

so i just stopped. put the ont back to its normal factory pppoe mode, unplugged the
r480t+ from everything, and left the network exactly as it was before i touched anything.
no harm done, just a wasted evening and a plan that needed rethinking.

this is actually the part of the whole project i'm most okay with documenting honestly —
it didn't work first try, i hit a wall i genuinely didn't have an immediate answer for,
and i backed out clean instead of pushing forward blind. the actual fix for this came
later, and it ended up being a better design than the original plan anyway.
