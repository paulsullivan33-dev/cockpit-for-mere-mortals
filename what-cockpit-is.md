# What Cockpit is

After this chapter you will be able to say, in one breath, what
Cockpit does, what it deliberately does not do, and why the
security rule in this chapter is not decoration.

**Cockpit** is a web console for a Linux server. You open a
browser, go to the server's address at **port 9090** (a *port* is
a numbered door on a machine's network address; services listen
on different doors), log in with your ordinary Linux username
and password, and get a control panel for that machine: its
services, its disks, its network, its users, its logs, and a
full terminal, all in the browser.

Cockpit is built and maintained by the Cockpit Project, a team
working in the open, with a large share of its developers at Red
Hat. It is free software, installed from your distribution's own
packages, and it is a standard part of Red Hat Enterprise Linux,
Fedora, CentOS Stream, Debian, and Ubuntu alike. When people say
"the web console" about a modern Linux server, this is usually
what they mean.

Three facts about Cockpit matter more than any feature list.

**It uses the machine's own machinery.** Cockpit does not keep
its own copy of your system's state. When it shows you a
service, it is asking **systemd** — the program that starts,
stops, and supervises services on a Linux machine — for the real
answer, at that moment. When it changes a setting, it uses the
same system interfaces and configuration files you would use
from the command line. That is why Cockpit and the terminal
never disagree, and why nothing breaks if you stop using
Cockpit: there is no database to export and no agent to remove.

**It logs in as you.** There is no separate "Cockpit admin"
account and no Cockpit password store. You log in with your
Linux account, through the same **PAM** (Pluggable
Authentication Modules — the standard sign-in gatekeeper on
Linux) as SSH. Your permissions are your permissions: an
ordinary user sees mostly read-only views, and an administrator
— someone allowed to use **sudo**, the command that runs one
action with the system owner's authority — can click
"Administrative access" inside Cockpit and do administrator
work. Actions are taken in your name, and the system log records
them that way.

**It is idle until you visit.** On a normal install, Cockpit's
web service is *socket-activated*: systemd holds the port open
and only starts the program when a browser actually connects.
When nobody is looking at the console, almost nothing is
running.

Now the rule, stated plainly, because this tool has real power:

> **Cockpit is a set of master keys hung on a web page.** Anyone
> who logs in as an administrator can stop services, wipe disks,
> create accounts, and open a root terminal. So: install it only
> on machines you own or are responsible for administering.
> Reach it only on networks you trust — your home or office
> network, or through a VPN or SSH tunnel from outside. Give
> every account a real password. And keep the machine updated,
> because a patched Cockpit on a private network is a tool, and
> an unpatched one facing the public internet is an invitation.

This guide follows that rule throughout. Every example uses
practice machines, invented addresses, and accounts made up for
the page. Nothing here needs, or should ever be pointed at, a
machine that is not yours to administer.

What Cockpit is *not*: it is not a monitoring system (it shows
the present, and a short recent history of performance graphs;
it does not watch your machines while you sleep or alert you —
the *Watching Your System for Mere Mortals* guide covers tools
that do). It is not a configuration-management system (it changes
one machine at a time, by hand; it does not declare the state of
a hundred). And it is not a replacement for knowing the command
line — it is, at its best, a well-lit room full of the same
controls the command line drives.
