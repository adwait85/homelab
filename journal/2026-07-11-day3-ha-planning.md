# day 3 — the nervous part, and finding the backup node

hadn't actually started setting anything up on the real network yet at this point. was
stuck on a genuinely uncomfortable thought — if i do this and something goes wrong,
software issue, hardware issue, whatever, i could take the entire home network down.
and worse, i wasn't experienced enough yet to even know how to fix it fast. that's not
just "oops no internet for an hour," that's potentially a proper mess for everyone in
the house depending on it.

so before touching anything live, i started thinking about what a "safe" version of
this would even look like. if i'm building my own router, what does a real high
availability setup for that actually mean.

started reading into it and the "correct" answer for serious homelabs seemed to be a
3-node proxmox cluster. quickly did the math on that and no — 3 systems was pushing it
hard on cost for what this needed to be. scaled the ambition down to something more
reasonable: i just need one backup for opnsense specifically, not a whole cluster.

that's when i came across CARP. quick explanation for anyone reading this who hasn't
run into it — CARP (Common Address Redundancy Protocol) basically lets two firewalls
share one virtual IP address between them. one is active (master), the other sits there
doing nothing traffic-wise but constantly checking in on the master. the moment the
master stops responding, the backup takes over that same virtual IP within a couple of
seconds, and every device on the network just keeps working since they were only ever
talking to the virtual IP anyway, not the physical box directly. genuinely elegant once
it clicks.

so now i needed a second, smaller system to actually host that backup opnsense instance.
a thin client felt like exactly the right fit for this — low power, small, doesn't need
to be powerful since it's just sitting idle most of the time waiting to take over.

found one on indiamart, the kind that usually gets phased out of exam centers once
they upgrade. intel celeron, 4th gen i think, 4gb ram — perfectly fine for what carp
needs. picked it up for 1600rs.

only issue — it came with a 2 pin power adapter. no earth pin. and touching the back
of the thing genuinely gave a small shock-like feeling, exactly the same leakage current
thing i ran into later with the switch's adapter too. same root cause, no ground path for
the leakage to go anywhere except through me. fixed it by just buying a proper 12v 5a smps
separately off amazon for 350rs instead of trusting the bundled adapter.

with the hardware side sorted, moved on to software. wasn't familiar with proxmox at all
at this point, so my first instinct was "eh, any linux distro will probably do" and
installed zorin os on it. ended up doing almost as much research fighting zorin's
limitations for this use case as i would've just learning proxmox properly in the first
place. so scrapped that, actually looked into proper hypervisor/server os options this
time, and installed proxmox on both the main server and the thin client instead.

last thing — realized i needed a lot more cat6 cable than i had lying around. had a
decent stock already but a chunk of it turned out to be cat5, not cat6, so picked up
more to make sure everything running vlans and gigabit links actually had cable that
could keep up.

that was everything sorted on the planning and parts side. next part's where i actually
start touching the live network.
