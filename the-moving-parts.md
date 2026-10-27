# The moving parts

After this chapter you will be able to name the pieces Cockpit
is made of, say which one you are talking to at each moment, and
know where its files and settings live.

Cockpit is small, and it is honest about its parts. Four pieces
do almost all of the work.

**cockpit-ws — the web service.** This is the program that
serves the login page and the console pages to your browser. It
listens on port 9090, speaks HTTPS (encrypted web traffic; the
encryption is explained in the install chapter), and checks your
username and password through PAM when you log in. It does not,
itself, know anything about your disks or services. It is a
doorman and a translator, not the building.

**cockpit-bridge — the go-between.** When your browser asks a
question — "how much memory is used?", "restart this service" —
the bridge is the program that carries it to the machine. One
bridge runs per logged-in user, with exactly that user's
permissions. The bridge talks to the system's real machinery:
systemd for services, **D-Bus** (the message bus programs on a
Linux machine use to talk to each other) for most requests, and
plain files under `/proc` and `/sys` for measurements. If the
bridge cannot do something as your user, the answer is no —
Cockpit has no back door around it.

**The bridge's helpers.** Some jobs are handed to small
specialist programs. A **storage** helper talks to **udisks2**,
the disk-management service. A terminal in the browser is a
real shell started for you by a session helper. When you ask for
administrative access, a sudo-backed bridge is started alongside
yours, and privileged work flows through it. Each piece runs
only while it is needed.

**The pages — "applications".** The console you see is a set of
small web applications, each living in its own folder under
`/usr/share/cockpit/`: `shell` (the console frame and the
overview page), `systemd` (services and logs), `storaged`
(disks), `networkmanager` (network), `users` (accounts),
`packagekit` (updates), and the terminal, which ships with the
base pages. This is why Cockpit is easy to extend —
chapter 12 is about add-on applications that drop into the same
place — and why a minimal machine can install only the pages it
needs.

Around those four, a few supporting facts:

- **The socket.** A systemd *unit* is a named description of a
  piece of work systemd manages. `cockpit.socket` is the unit
  that listens on port 9090 and starts `cockpit.service` (the
  unit that runs cockpit-ws) when a connection arrives. On a
  healthy install, "is Cockpit up?" usually means "is the
  socket enabled?", and chapter 3 checks exactly that.
- **No database.** Settings live in ordinary places:
  `/etc/cockpit/` holds the small amount of Cockpit's own
  configuration, including any TLS certificate you give it.
  Everything else is the system's own state, read live.
- **Other machines.** The same bridge protocol can travel over
  SSH. That is how one Cockpit session can show several
  machines: the first machine's console asks the others, over
  SSH, to start their bridges. Chapter 11 builds on this.
- **The browser does the drawing.** Graphs, tables, and forms
  are rendered by JavaScript in your browser, from data the
  bridge supplies. A slow console page is usually a slow
  browser tab or a busy machine, not a broken Cockpit.

A useful habit from here on: when something misbehaves, ask
*which part* would produce that symptom. Login page never
appears — the socket or the web service. Login works but a page
is empty — that page's helper or the service behind it
(udisks2, NetworkManager) is missing or stopped. A button is
greyed out — your bridge lacks the permission, and the answer
is administrative access, not a reinstall. The troubleshooting
chapter is organised exactly this way.
