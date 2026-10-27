# The neighbours

After this chapter you will be able to say how Cockpit differs
from the tools it is most often confused with — Webmin,
Portainer, and the plain SSH session it is accused of
replacing — and when each is the right tool.

## Webmin — the older web console

**Webmin** has been administering Unix-like systems from a
browser since 1997, and it is Cockpit's most direct neighbour:
one web login, pages for users, disks, services, packages,
and nearly everything else. The differences are architectural
and they matter.

Webmin is a large program of its own, with its own web server,
its own accounts (which can map to Linux users, but are its
own layer), and its own modules that read and write system
configuration files directly. It is broader than Cockpit —
it will happily configure services Cockpit has no page for —
and it is correspondingly heavier: a Webmin install is always
running, and its long history includes serious security holes
in that always-on surface, which its maintainers do patch
diligently. Cockpit, by contrast, logs in *as you* through the
system's own gatekeeper, idles until visited, keeps no state,
and uses the system's own tools — a smaller, duller design,
which is precisely its defence. The honest rule: if a machine
already runs Webmin well and its administrator knows it, it is
a fine tool; for a new machine today, Cockpit is the one that
ships with the system, follows its rules, and leaves nothing
behind when removed. (Usermin and Virtualmin are Webmin's
siblings — per-user webmail and files, and web-hosting
management; different jobs again.)

## Portainer — the container console

**Portainer** is a web console for **containers** — Docker
(and Kubernetes) workloads: stacks, images, volumes, networks,
container logs and consoles. It is not a general server
console at all; it administers the container engine the way
Cockpit administers the machine. That makes the two
neighbours on the same street rather than rivals for the same
house: a typical small server runs containers *under* an
operating system that still has disks, users, services, and
updates — Portainer's world sits inside Cockpit's. If your
mental question is "is my application up?", Portainer (or
Cockpit's own Podman page, from chapter 12) is the nearer
tool; if it is "is the *machine* all right?", that is
Cockpit's question. This series' own Portainer guide covers
the container side properly.

## Plain SSH — the tool Cockpit is not replacing

The third neighbour is not a web console at all. An **SSH
session** gives you the machine's shell directly: your keys,
your terminal, every tool ever installed, and nothing in
between. Against it, Cockpit's case is not power — SSH wins
power; the terminal chapter said so — but *presentation*:
the graph instead of the number, the failed-services count
you cannot fail to notice, the disk map instead of a column of
figures, the form that shows the bond's members while you
build it. And SSH's case against Cockpit is equally real:
no web service to patch, no browser quirks, works at any
bandwidth, scriptable end to end.

Most administrators who use Cockpit at all end up bilingual:
SSH for the terminal-shaped jobs and the automation, Cockpit
for the look-at-the-machine jobs — the health check, the disk,
the "what is this server even doing" question from a machine
they do not usually touch, the colleague who needs a window
but not a shell. The tools do not compete. The only losing
move is having exactly one door into a machine you care about,
whichever door it is.
