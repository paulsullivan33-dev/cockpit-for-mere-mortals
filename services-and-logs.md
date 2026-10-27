# Services and logs

After this chapter you will be able to find a service in
Cockpit, read its state, restart it safely, make it start at
boot — and use the Logs page to answer the question every
administrator asks first: *what just happened?*

## The Services page

A **service** is a program the machine runs in the background —
a web server, a database, SSH — started and supervised by
systemd. The Services page is systemd, drawn.

Open **Services** from the menu. You get a list of units
grouped by kind, with tabs worth knowing by name:

- **Services** — the background programs themselves.
- **Targets** — named bundles of services that describe a
  state of the machine (like "multi-user mode"). Mostly you
  look at these; rarely you touch them.
- **Sockets, Timers, Paths** — the triggers that start services:
  a socket starts a service when a connection arrives (this is
  how Cockpit itself starts), a timer starts one on a schedule
  (the systemd answer to cron), a path starts one when a file
  changes.

Each row shows the unit's name, a one-line description, and its
state. The two states people confuse:

- **Active / inactive / failed** — is it running *now*? *Failed*
  is shown plainly, and the overview page counts failed units
  so you cannot miss them.
- **Enabled / disabled** — will it start *at boot*? These are
  independent: a service can be running but not enabled (it
  will not come back after a reboot) or enabled but stopped.

Click a service to open its page. At the top: its state, buttons
to **Start**, **Stop**, and **Restart**, and a switch or menu
for its boot behaviour — enabling or disabling it. Below: the
unit's relationships (what it needs, what needs it) and the
most recent lines of its log, which is usually where the truth
is. Changing anything here needs administrative access; without
it the page is a window, not a control panel, and that is by
design.

One discipline worth adopting: **restart, don't stop-and-start,
and read the log first.** A service that failed an hour ago and
was never read is a mystery you chose. The service page shows
you the failure, in place, in the log panel at the bottom —
read the last few lines before you touch the buttons, because
restarting erases nothing but teaches nothing either.

Everything on this page is systemd told through D-Bus, and
everything you do here can be done (and undone) from the
terminal with `systemctl` — the *systemd for Mere Mortals* guide
is the same country from the other side of the border.

## The Logs page

The **journal** is the machine's single, unified log: every
service's output, every login, every kernel message, in one
stream, timestamped. The Logs page is a reader for it.

Open **Logs**. Across the top are the controls that matter:

- **Time range** — last hour, last 24 hours, last 7 days, boot,
  or a custom span. "Since boot" is the usual starting point
  after a reboot surprise.
- **Priority** — how serious a message must be to be shown, from
  *Everything* up through *Warning*, *Error*, and *Emergency*.
  Filtering to **Errors and above** is the fastest health check
  a machine offers: a short list is a healthy machine; your own
  name for the entries you recognise is the audit.
- **Text search and identifiers** — a search box for words, and
  filters by the program that wrote the entry. Combined with
  the service page's per-service logs, this covers both "what
  did *this program* say?" and "what did *everything* say at
  3 a.m.?"

Entries expand to show their full fields — the writing program,
the exact timestamp, the systemd unit, the boot it belongs to.
There is a follow/pause behaviour for watching new entries
arrive, which turns the Logs page into a tail-gate for whatever
you are about to try in another tab: change something, watch
what the machine says about it. That loop — act in one page,
read in Logs — is the daily rhythm of administering through
Cockpit, and it is the same loop as `journalctl -f` in a
terminal, with better lighting.
