# homelab
My journey into networking
just so nobody's confused scrolling through the journal folder — the "day 1, day 2"
thing isn't a live build log written while i was actually doing the work. the whole
project's already done and running. i just liked the idea of documenting it this way
after seeing a few people do something similar on youtube, so i went back and wrote it
up in that day-by-day style purely for readability, not because i sat down and typed
each entry the same day it happened.

so treat it as a written-after-the-fact walkthrough of everything that happened, broken
into parts so it's not one giant wall of text, not an actual diary.

what's actually running

proxmox host running opnsense as the main router, casaos (adguard, matrix, jellyfin and
a few other things), home assistant, a hikvision nvr with 8 cameras, a thin client slowly
turning into a second node for failover, all tied together with a managed switch doing
proper vlan separation.

started as "i just want network-wide ad blocking" and did not stay that way for long.
## Build Log

- [Day 1 — Origin Story](journal/2026-07-03-day1-origin-story.md)
- [Day 2 — The Parts Hunt](journal/2026-07-06-day2-parts-hunt.md)
- [Day 3 - The Nervous Part](journal/2026-07-11-day3-ha-planning.md)
- [Day4 - pppoe-failure](journal/2026-07-12-day4-pppoe-failure.md)
- [Day5 - the pivot](journal/2026-07-14-day5-the-pivot.md)
