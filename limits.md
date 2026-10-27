# Limits

After this chapter you will be able to name what Cockpit
cannot do — before you rely on it for one of those things.

**It is one machine at a time, by hand.** Chapter 11's host
switcher makes twenty machines reachable; it does not make
them one system. Every change is still made on one host, in
one session, by a person clicking. There is no way to say
"these fifty machines shall all have this setting" and know
it is so — that is configuration management (tools like
Ansible declare a desired state and enforce it), and it is a
different discipline with its own guide in this series.
Cockpit's honesty about this is a feature; believing otherwise
about it is the failure mode.

**It shows the present, and only a little of the past.** The
overview graphs hold a short recent history. Cockpit does not
keep long performance records, does not watch while you are
away, and will never alert you that a disk filled at 3 a.m.
Monitoring is a different job, done by different tools
(Prometheus and Grafana are this series' example), and a
healthy setup uses both: the monitor notices, then a human
opens Cockpit to look.

**It only manages what its helpers manage.** The pages are
windows onto systemd, NetworkManager, udisks2, PackageKit.
Where a machine is run by other machinery — networking
configured by hand or by a cloud's tooling, software installed
outside the package system, services supervised by something
else — the corresponding page is empty or partial, and the
fix is not in Cockpit. It draws the system's map; it does not
redraw the territory.

**The power is the risk, restated without decoration.** A
Cockpit login with administrative access is a root shell with
buttons. That means: a stolen password for an administrator
account is the whole machine; a Cockpit left reachable from
the public internet will be found, and its login page knocked
on, by strangers, forever; and a browser session left logged
in on a shared desk is an open door. The mitigations are the
unfashionable ones from chapter 1 — private networks or a VPN,
real passwords, prompt updates, accounts that are
administrative only for the people who must be — and they are
not optional extras. Cockpit's design (socket activation, PAM,
no stored secrets, per-user bridges) reduces the surface; it
cannot reduce your habits.

**It is a web page, with a web page's dependencies.** The
console needs a modern browser, JavaScript, and a connection
that can hold a session. Over a dreadful link, SSH will still
be working long after the console has become an exercise in
patience. And the interactive pages, by their nature, resist
automation: you cannot script a click. Everything Cockpit
does *can* be done by scriptable tools — that is its design —
but you will be scripting *those*, not Cockpit.

**Some of the machine is simply not in it.** Boot loaders,
partition tables below the filesystem's floor, firmware, the
BIOS-era world: Cockpit starts where a running Linux system
starts. And a machine that will not boot has no port 9090 to
visit — which is why the rescue console and the physical
keyboard remain, permanently, part of the toolkit this
console can assist but never replace.
