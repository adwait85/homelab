# day 2 — the parts hunt

okay so like i said, this started as a pi-hole idea basically. just wanted network wide ad
blocking, that was genuinely the whole plan. but once i started researching that, youtube's
algorithm did what it does and i kept getting fed videos about people building their own
routers, running opnsense, doing vlans, the whole thing. and i just fell into it. by the
time i came out the other side of that rabbit hole the plan had quietly turned into "build
a proper router + server setup" instead of "block some ads." no regrets though.

## the motherboard deal

found a guy on reddit clearing out space, selling a gigabyte B75M board with dual bios, 4
ram slots, 2 big pcie slots and 2 small ones, bundled with a xeon (4 core 4 thread) and
8gb ddr3 ram. all of it together for 2500rs.

genuinely felt like a good deal and also just a considerate one on his part — ram prices
alone for just 8gb ddr3 are sitting around 1700rs right now, so the rest was basically
next to nothing on top of that. and this wasn't some random h61 or h81 budget board either,
those usually only give you 2 ram slots and barely any expansion. 4 ram slots + 4 pcie
slots on a b75 chipset is actually solid for what I wanted to do with it later — passthrough
cards, dual nics, all of that needs slots.

## realizing i wanted to build my own router

this is where the intel nic comes in. bought an intel dual-port server nic off indiamart
for 2200rs. at this point the plan had fully shifted — wasn't just going to be a
pi-hole box anymore, i wanted actual router hardware, proper wan/lan separation, the
whole opnsense route.

alongside that, browsing olx one day i came across an 8-port tp-link gigabit managed
switch for 1000rs. grabbed it immediately, obviously needed for vlans anyway.

and then completely unplanned — still browsing olx, saw a tp-link tl-r480t+ listed for
2000rs, which is about half its usual price. i genuinely had no concrete need for it at
that point. just saw a good deal and figured some use would come eventually. only later
realized it's not even gigabit, just 10/100 ports. bought it anyway, no regrets there
either, it's ended up being useful for random side stuff since.

## the storage saga

storage is expensive right now, so obviously went secondhand first. got a 240gb sata ssd
off someone. and got scammed. crystaldiskinfo showed 100% health, looked completely fine
on paper, but i literally could not open it or move a single file off it. spent a while
trying to fix it — tried changing the drive's firmware, tried a few different tools —
turns out this is apparently a known thing, some bad drives get stuck in this exact kind
of lock state where health reporting still looks perfect but the drive's actually cooked.
nothing worked.

gave up on that one and just bought new instead — a 128gb sata ssd from Consistent, off
amazon, for 2000rs. smaller than i wanted but at least it actually works.

## psu, gpu, case

psu: ant esports 450w for 1500rs.

gpu: used amd radeon r7 260x for 2000rs. needed this purely because the xeon has no
integrated graphics at all, so without a gpu there's literally no display output, can't
even get into bios. so this was non-negotiable just to get the thing alive. in hindsight
though, i probably should've looked into a slightly newer card instead — video decode
support matters more than i realized back then, and this one's going to need workarounds
down the line for hardware-accelerated stuff. but it did exactly the one job i needed at
the time, which was just "give me a picture on screen."

case: foxin micro atx, 1200rs, from a local shop.

## putting it together

assembled all of it myself, with a bit of help from my dad. worth saying — this was
genuinely the first time in my life i'd actually seen the inside of a real desktop PC.
Arduino boards and dev kits are one thing, this was a different scale of "please don't
break the expensive thing" energy. but it went together fine in the end.

that's the hardware side sorted. next part is where the actual network design starts.
