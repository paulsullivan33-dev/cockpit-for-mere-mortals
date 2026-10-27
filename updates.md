# Keeping the machine updated

After this chapter you will be able to see what updates a
machine is waiting for, apply them from the console, and set
the machine to keep itself current — the chore that chapter
1's security rule quietly depends on.

Open **Software Updates**. The page is drawn by
`cockpit-packagekit`, and it talks to **PackageKit** — a layer
that gives different distributions one shared way to answer
"what updates exist, and please install them". Underneath,
your distribution's own machinery still does the work: `apt`
and `dpkg` on Ubuntu and Debian, `dnf` and `rpm` on Fedora and
RHEL. Cockpit is a window onto those tools, not a replacement
package system, and packages it installs are ordinary packages
your command line can see, verify, and remove.

## Reading the page

The page lists the updates that are available, grouped so the
important ones stand out:

- **Security updates** are shown first and counted separately.
  These are the fixes for holes someone could walk through;
  on any machine reachable by other people, they are the ones
  that do not wait for a convenient weekend.
- Ordinary updates — bug fixes and new versions — follow, each
  with the package name, the version change, and, where the
  distribution provides it, a short description of what changed.

If the machine is fully current, the page says so and means
it: the check is live, against the machine's own package
sources. A mostly-idle server's usual state is a handful of
waiting updates; a long list on a machine you thought was
maintained is information, and it is free.

## Applying updates

With administrative access, an **Install all updates** button
does what it says. The page shows progress as packages
download and install, and tells you plainly at the end if a
**restart** is needed — some updates (a new kernel, the core
system library) only take effect when the machine reboots, and
the page surfaces that fact instead of letting you believe you
are patched while running the old code. There is a restart
button beside the warning. Use the page's honesty: an unapplied
restart is an unapplied update.

## Automatic updates

The page has a settings area for **automatic updates**:
whether to install updates without being asked, which kind
(security only, or everything), and on what schedule. Set a
server to take its security updates automatically and you have
removed the largest single risk a small machine carries —
being the one that missed the famous patch. Two honest
qualifications: automatic updates still respect the restart
rule above (they install; they do not reboot a production
machine out from under its work — check in now and then and
restart deliberately), and an update can occasionally be the
thing that breaks a service, which is why reading the Logs
page after a round of updates is a habit, not a superstition.

For Ubuntu and Debian readers: the machinery under this page
is the same `unattended-upgrades` system the command line
configures, and the console's settings and the files under
`/etc/apt/` describe the same behaviour. RHEL-family machines
use `dnf-automatic` in the same role. One system, two doors —
which is, by now, a familiar shape.
