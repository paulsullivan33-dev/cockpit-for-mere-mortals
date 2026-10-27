# Your first login

After this chapter you will have logged in to Cockpit for the
first time, found your way around the overview page, and turned
on administrative access — the three things every later chapter
assumes.

This chapter is taught from the interface as documented and as
this guide's lab verified at the door: the lab confirmed the
login page answers on port 9090 (see `criteria.md`), but the
clicks below are described, not screenshotted, and the guide
says so wherever it matters.

**Before the browser: three checks on the machine itself.**

```console
$ systemctl status cockpit.socket
$ ss -tlnp | grep 9090
$ curl -k https://localhost:9090/
```

The socket should be *active (listening)*, something should be
listening on port 9090, and `curl` should get an answer — an
HTTP status and some HTML, not a connection refused. The `-k`
flag tells curl not to insist on verifying the certificate yet;
the next paragraph explains why that is expected, this once, on
your own machine.

**The address.** In the browser, go to
`https://your-server:9090`, using the machine's name or address
on your own network. The connection is encrypted (the *s* in
https), using a **TLS certificate** — a small signed document
that lets the browser encrypt the conversation and check it is
talking to the machine it thinks it is. On a fresh install,
Cockpit generates its own certificate the first time it starts
and stores it in `/etc/cockpit/ws-certs.d/`. Because that
certificate is *self-signed* — vouched for by the machine
itself, not by a public authority your browser already trusts —
the browser will warn you, once, that it cannot verify the
connection. On your own machine, reached at an address you typed
yourself, that warning is the expected shape of a first visit:
check the address is right, then proceed. On any other machine's
warning — one you did not just install, at an address you did
not type — stop. That distinction is the whole game with
certificates, and the troubleshooting chapter returns to it in
earnest. (You can replace
the self-signed certificate with one of your own later;
`/etc/cockpit/ws-certs.d/` is where they live, and Cockpit
picks the newest valid one.)

**The login page.** Cockpit asks for the username and password
of an account *on that machine* — the same ones you would use
for SSH or sudo. There is nothing new to memorise and nothing
Cockpit-specific to create. Log in as your normal user, not as
root: Cockpit is designed for exactly that, and the next step
is how the power arrives.

**The overview.** You land on the System overview: the
machine's name, its hardware and operating system, health cards
for CPU, memory, disk, and network with short recent-history
graphs, and any complaints the machine has — failed services,
available updates — surfaced as links. Down the left side is the
menu of pages this guide spends a chapter each on: Services,
Logs, Storage, Networking, Accounts, Terminal, Software Updates.
At the top, your username; beside it, later, the host switcher
of chapter 11.

**Administrative access.** Look at the top of the page: if you
logged in as an ordinary user who may use sudo, Cockpit offers
a button labelled **Administrative access** (in some versions it
says "Limited access" with a button to unlock). Click it, type
your password again, and the console's read-only views become
working controls — services can be restarted, disks changed,
accounts created. This is sudo, wearing a web page: the actions
happen as root, they are logged as yours, and the access lasts
for the session. Almost everything in the next seven chapters
needs it, and almost nothing on the overview does — which is
the right default, and worth copying in your own habits: browse
as a user, elevate when you mean it.

**Your first useful act.** Click **Services**, find one you
recognise, open it, and read its page — state, recent log
lines, start/stop controls — without changing anything. Then
open **Terminal** from the menu and run `whoami` and `hostname`
in the browser tab. You have now used the two halves of Cockpit:
the purpose-built pages, and the machine itself, one click away.
Chapter 5 starts with the Services page properly.
