# Glossary

The terms used in this guide, collected in one place. Each
definition is the working one the chapters relied on.

**Administrative access** — Cockpit's unlocking of sudo power
inside a session: a button, your password again, and the
console's read-only views become working controls, logged as
yours.

**Bond** — several network links joined to act as one, for
speed or for survival when a cable fails.

**Bridge (network)** — a software switch joining interfaces
into one network segment.

**cockpit-bridge** — the Cockpit program that runs per user
and carries requests from the console to the system's own
machinery — systemd, D-Bus, the files under `/proc` — with
exactly that user's permissions.

**cockpit.socket** — the systemd unit that listens on port
9090 and starts the web service only when a browser connects.
Cockpit's normal state: enabled socket, idle everything else.

**cockpit-ws** — the web service: serves the login page and
the console pages on port 9090, checks passwords through PAM.

**D-Bus** — the message bus by which programs on a Linux
machine talk to each other; the bridge's usual road to system
services.

**DHCP** — the network's address-clerk: hands a machine its
IP address, gateway, and DNS servers when it joins a network.

**Firewalld / ufw** — the two usual Linux firewall managers.
Cockpit's Networking page can drive firewalld; ufw is managed
from its own command line.

**Host switcher** — the machine-name menu at the top left of
the console, where other machines are added and selected.
Read it before consequential clicks.

**Journal** — the machine's unified log, kept by systemd:
every service, login, and kernel message in one timestamped
stream. The Logs page reads it; `journalctl` is its command.

**LVM** — a storage layer that pools disks into volume groups
and divides them into logical volumes, so storage can be
resized and rearranged without raw partitions.

**Mount** — attaching a filesystem at a directory so its files
appear there. An unmounted filesystem is present but invisible
to ordinary programs.

**NetworkManager** — the service that configures network
interfaces on most distributions; the Networking page is a
form laid over it.

**PackageKit** — a layer giving distributions one shared way
to answer "what updates exist, and install them". The Software
Updates page talks to it; apt or dnf still does the work.

**PAM** — Pluggable Authentication Modules, the standard
sign-in gatekeeper on Linux. Cockpit has no passwords of its
own; PAM checks yours.

**Port** — a numbered door on a machine's network address.
Cockpit's is 9090; SSH's is 22; the web's are 80 and 443.

**Pseudo-terminal** — the software stand-in for a physical
terminal that shells and full-screen programs expect; the
Terminal page attaches your real shell to one.

**RAID** — several disks acting as one volume, usually with
redundancy so a disk can fail without the data following it.

**Self-signed certificate** — a TLS certificate vouched for by
the machine itself rather than a public authority. Browsers
warn about them on first sight; on your own machine, at your
own address, that warning is the expected first-visit shape.

**Service** — a background program systemd starts, supervises,
and stops: a web server, a database, SSH itself.

**Socket activation** — systemd holding a port open and
starting the program behind it only when a connection arrives.
Why an idle Cockpit runs almost nothing.

**Sudo** — running one action with the system owner's
authority. Cockpit's administrative access is sudo through a
web page, with the same logging.

**Systemd** — the program that starts, stops, and supervises
services on a modern Linux machine, and keeps its journal.
Cockpit's Services page is systemd, drawn.

**TLS certificate** — the signed document that lets a browser
encrypt its conversation with a server and check the server's
identity. Cockpit keeps its in `/etc/cockpit/ws-certs.d/`.

**Udisks2** — the system's disk-management service; the
Storage page works through it.

**Unit** — a named description of a piece of work systemd
manages: a service, a socket, a timer, a target.

**VLAN** — tagged network traffic that lets one cable carry
several separate networks at once.
