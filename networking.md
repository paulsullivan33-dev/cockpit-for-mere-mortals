# Networking

After this chapter you will know what the Networking page can
change, what it cannot, and how not to lock yourself out of a
machine whose network settings you are editing through the
network.

The Networking page works through **NetworkManager**, the
service that configures network interfaces on most desktop and
many server Linux distributions. If your machine's networking
is managed by something else — a cloud image's own tooling,
hand-written configuration, systemd-networkd — this page will
show you less, or politely nothing. That is not Cockpit
failing; it is Cockpit only speaking to the manager that is
actually in charge.

## What the page shows

At the top, live graphs of traffic in and out. Below, one card
per **interface** — the machine's network connections: ethernet
ports, Wi-Fi, and virtual ones. Each shows its state, its
hardware (MAC) address, and its addresses: the IPv4 address
(such as 192.168.1.50, with its subnet size), any IPv6
addresses, the default gateway (the address that forwards
traffic onward, usually your router), and the DNS servers
(the directories that turn names into addresses).

Click an interface for its settings: whether its address is
automatic (assigned by **DHCP**, the network's address-clerk,
when it connects) or manual, and if manual, the address, gateway,
and DNS entries to edit. There is a switch to turn an interface
off and on — read the lockout warning below before using the
one you arrived through.

## What you can build here

- **Bonds** — several physical links acting as one, for speed
  or for survival if a cable dies.
- **Bridges** — a software switch that connects interfaces (and
  virtual machines) into one network segment.
- **Teams and VLANs** — the enterprise variations: teaming is
  a newer kind of bond; a VLAN tags traffic so one cable can
  carry several separate networks.

Each is created from the buttons at the top of the interface
list, configured in a form, and applied live. These are the
same objects `nmcli` builds from the terminal; the page is a
form laid over NetworkManager, and it keeps no secrets from it.

## The firewall section — and an Ubuntu-shaped caveat

On systems running **firewalld**, the page includes a Firewall
section: the firewall's zones, and switches to open or close
named services (SSH, HTTP, and friends) through it. On Ubuntu,
whose usual firewall is **ufw**, that section does not manage
your rules; ufw stays a command-line tool (there is a guide for
it in this series). Know which firewall your machine actually
runs before hunting for a switch that is not there.

## The lockout warning, in bold because it earns it

**You are very likely reading Cockpit through the network you
are about to change.** Give the interface a wrong static
address, turn it off, or break its gateway, and your browser's
connection dies mid-click — usually fixable, but only from the
machine's own keyboard, or through whatever out-of-band access
the machine has. So: before editing the interface you arrived
on, be sure you have another way in — a physical console, a
second network, a cloud provider's rescue console. Change one
thing at a time, and write down the address you started with.
SSH has exactly the same property, and it is worth remembering:
the difference is that Cockpit makes the dangerous click look
like every other click.
