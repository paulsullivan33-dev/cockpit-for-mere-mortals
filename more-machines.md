# Adding more machines to one console

After this chapter you will understand how one Cockpit tab can
administer several machines, what the other machines need
installed (much less than you would guess), and what actually
travels between them when you switch hosts.

Everything so far has been one browser, one machine. But the
bridge protocol of chapter 2 travels well: it can run over an
SSH connection. So a single Cockpit session, hosted on one
machine, can ask other machines to start their bridges over
SSH and show their pages in the same console, under the same
login. You administer the fleet — two machines, or twenty —
from one tab, switching between them from a list.

## What each machine needs

- **The host machine** (the one whose address you type into
  the browser) runs the full Cockpit: the web service, and any
  pages you use.
- **Each other machine** needs only enough Cockpit for its
  bridge to answer: the `cockpit-bridge` package and the page
  packages you want to see (`cockpit-system`, `cockpit-storaged`
  and friends). It does **not** need the web service, does not
  need port 9090 open, and does not serve a login page of its
  own. A machine you administer this way has no extra web door
  at all — one of the neatest security properties of the whole
  design.

Small print, stated honestly: the bridge must be new enough to
speak to the host's version, so keep the fleet's Cockpit
versions roughly in step, and if a machine's distribution is
very old, its bridge may be too old to join.

## Adding a machine

In the console, open the **host switcher** — the machine name
at the top left, beside the Cockpit menu. It lists the current
host and offers **Add new host**. You give:

- The machine's address — a name or IP address the *host
  machine* (not your browser) can reach over SSH. Worth pausing
  on: the SSH connection is made from the Cockpit host to the
  other machine, so the address must make sense from there.
- A username on that machine.

The first time, Cockpit shows you the other machine's **SSH
host key** fingerprint for confirmation — the same check SSH
itself makes the first time you connect anywhere. Confirm it
the way SSH taught you: it should match the key the machine
itself would show you (`ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub`
run on that machine). Then you log in to that machine with its
password (or your SSH key, if the host machine holds one the
other accepts), and it joins the list. From then on, picking it
in the switcher changes the whole console — every page, the
terminal included — to that machine, with the host name always
visible at the top so you know whose disks you are looking at.

That last point deserves emphasis, because it is the chapter's
one real hazard: **the pages look identical for every machine.**
The console shows the current host's name prominently, and the
switcher keeps the list visible, and still, the fleet
administrator's classic accident is restarting a service on
the production machine while believing they were looking at the
test one. Read the host name before every consequential click.
Professionals do this out loud, and so should you.

## What crosses the wire

Only the bridge protocol, over the SSH connection the host
opened: requests out, answers back. Your browser still talks
only to the one host. No machine needs to trust your browser,
and a machine can leave the fleet by nothing more dramatic
than the host forgetting it, or its SSH door closing. Remove a
host from the switcher's edit view when it retires, so the
list you trust stays the list that exists.
