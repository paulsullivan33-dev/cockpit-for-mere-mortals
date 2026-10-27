# Cockpit for Mere Mortals

Every Linux server already knows everything
about itself: which services failed, which
disk is filling, which updates it is
waiting for, who logged in and when. The
knowledge is all there — scattered across
a dozen commands, each with its own output
to read. **Cockpit** gathers it into one
place: a web console, served by the server
itself, that you open in a browser and log
in to with your ordinary Linux account.
Services, logs, disks, networks, users, a
terminal, and updates, drawn as pages
instead of command output — and every
action taken through them is taken by the
machine's own tools, as you, in the open.

This guide explains how that works, how
to install Cockpit from your system's own
packages, how to log in for the first
time and turn on administrative access,
and then walks the pages one at a time:
services and logs, storage, networking,
accounts, the built-in terminal, and
updates. It then covers the two things
that make Cockpit more than a convenience:
one console administering many machines,
and add-on pages — containers, virtual
machines, a file browser — that install
like any other package.

The rule that governs everything here is
stated in chapter 1 and never relaxed:
**Cockpit is an administration console
with real power, and it belongs on
machines you own or administer, on
networks you trust, behind real passwords,
kept up to date.** A login with
administrative access can stop services,
erase disks, and open a root shell. On
your own network that is the point of the
tool. Facing the public internet, it would
be an invitation, and this guide does not
teach that.

The lab behind this guide ran the real
thing: **Cockpit 314**, installed from
Ubuntu 24.04's own repositories on a small
Linux machine with systemd in charge,
exactly as chapter 3 describes. The lab
enabled the socket, proved systemd itself
was holding port 9090, proved the web
service was *not* running until asked,
then asked: an HTTPS request to the
machine's own loopback address woke the
service, which generated its self-signed
certificate on first start and answered
with the login page — the whole
socket-activation story from chapter 2,
verified end to end. The installed pieces
were inventoried on disk: the web service,
the bridge, and the page folders under
`/usr/share/cockpit/`, named in the
transcripts.

What the lab could not do, it did not
fake. The interactive console itself —
the clicking through services, disks, and
settings in a browser — is taught from
the project's documentation rather than
demonstrated, because a browser session
leaves no transcript worth keeping, and
this series does not claim clicks it did
not make. The chapters say so where it
matters, and `criteria.md` keeps the full
ledger of what was run and what was only
read.

## What this guide covers

- [**What Cockpit is**](what-cockpit-is.md) —
  the mental model: your own account, the
  machine's own machinery, and the security
  rule that is part of the tool
- [**The moving parts**](the-moving-parts.md) —
  the web service, the bridge, the helpers,
  and the pages, and how to tell which part
  a symptom belongs to
- [**Installing Cockpit**](installing-cockpit.md) —
  the distribution packages, the socket,
  the firewall, and the three checks that
  prove it works
- [**Your first login**](your-first-login.md) —
  the certificate warning judged properly,
  the overview page, and administrative
  access
- [**Services and logs**](services-and-logs.md) —
  systemd drawn as pages: states, restarts,
  boot behaviour, and the journal as a
  reader answers questions with
- [**Storage**](storage.md) — the disk map,
  and which buttons on it can destroy data
- [**Networking**](networking.md) —
  interfaces, bonds, bridges and VLANs,
  and the lockout that editing your own
  connection invites
- [**User accounts**](user-accounts.md) —
  creating people, the administrator
  checkbox, passwords, SSH keys, and
  retiring accounts kindly
- [**The built-in terminal**](the-terminal.md) —
  a real shell in a browser tab, and when
  to use it instead of SSH
- [**Keeping the machine updated**](updates.md) —
  PackageKit's window onto your real
  package system, security updates, and
  restarts that complete the job
- [**Adding more machines**](more-machines.md) —
  one console, many hosts, over SSH — and
  reading the host name before you click
- [**Add-on applications**](add-on-applications.md) —
  Files, Podman, and Machines: pages that
  arrive as packages
- [**Recipes**](recipes.md) — the Monday
  health check, the dying service, the
  full disk, and five more complete jobs
- [**The neighbours**](the-neighbours.md) —
  Webmin, Portainer, and plain SSH: what
  each does that Cockpit doesn't, and
  vice versa
- [**Limits**](limits.md) — one machine at
  a time, no monitoring while you sleep,
  and the power stated without decoration
- [**Troubleshooting**](troubleshooting.md) —
  the silent address, the certificate
  warning with two meanings, the refused
  login, the empty page
- [**Glossary**](glossary.md) — the terms,
  collected in one place

## What it doesn't cover

Exposing Cockpit to the public internet —
that is not a harder chapter of this book,
it is a misuse of the tool, and chapter 1
explains the line. Managing machines that
are not yours to administer is likewise
out: the whole design assumes your
machines, your accounts, your network.
Declaring the state of a fleet and
enforcing it automatically is
configuration management, a different
discipline with its own guide in this
series. Deep storage surgery, network
engineering beyond one machine's
interfaces, and the internals of how
Cockpit's protocol works belong to the
project's own documentation, which this
guide points to in spirit by teaching the
machine's concepts, not the protocol's.

## How to use this guide

Read chapter 1 for the model and the rule,
and chapter 2 before installing anything —
knowing which part holds what turns later
confusion into a named problem. Chapter 3
installs and chapter 4 logs in, in the
same sitting; do them on a machine you can
afford to learn on. Chapters 5 to 10 are
the pages, one at a time; read them in
order the first time, and afterwards in
whatever order the machine in front of you
demands. Chapter 11 is the one that changes
how you work if you keep more than one
server. The lab's transcripts live in
`cockpit-lab/transcripts/`; its notes are
`examples/lab-notes.txt` and the command
companion is `examples/commands.txt`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [systemd for Mere Mortals](https://paulsullivan33-dev.github.io/systemd-for-mere-mortals/)
- [Watching Your System for Mere Mortals](https://paulsullivan33-dev.github.io/watching-your-system-for-mere-mortals/)
- [SSH for Mere Mortals](https://paulsullivan33-dev.github.io/ssh-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/cockpit-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/cockpit-for-mere-mortals/blob/main/LICENSE-CODE).

