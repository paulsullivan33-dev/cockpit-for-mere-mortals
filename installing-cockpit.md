# Installing Cockpit

After this chapter, Cockpit will be installed on your machine,
its socket enabled and listening, and you will have proved all
three of those things with checks — before a browser is
involved at all.

Cockpit comes from your distribution's own repositories. There
is nothing to download from a website and no third-party source
to add. This guide's lab installed it exactly this way, on
Ubuntu 24.04, and got version **314**; the transcripts are in
`cockpit-lab/transcripts/`.

## Ubuntu and Debian

```console
$ sudo apt update
$ sudo apt install cockpit
$ sudo systemctl enable --now cockpit.socket
```

The first command refreshes your package lists. The second
installs Cockpit and its standard pages. On the lab's Ubuntu
that meant seven packages, and the list is worth recognising
because chapter 2's moving parts are in it: `cockpit-ws` (the
web service), `cockpit-bridge`, `cockpit-system` (the overview,
services, logs, accounts and terminal pages), `cockpit-storaged`
(storage), `cockpit-networkmanager` (networking), and
`cockpit-packagekit` (software updates), gathered by the small
`cockpit` package itself. The third command **enables** the
socket — arranges for it to start at boot — and starts it now.

A Debian note, from the project's own documentation: on
Debian, Cockpit is best installed from the *backports*
repository for your release (`apt install -t <release>-backports
cockpit`), which carries a current version where the main
repository's can be old. On Ubuntu the standard version is
current enough, and the lab used it.

## Fedora, RHEL, and CentOS Stream

```console
$ sudo dnf install cockpit
$ sudo systemctl enable --now cockpit.socket
$ sudo firewall-cmd --permanent --add-service=cockpit
$ sudo firewall-cmd --reload
```

Same shape: the package, the socket, and then the firewall —
on these systems `firewalld` is usually running, and the two
`firewall-cmd` lines open its named door for Cockpit.

On Ubuntu, the usual firewall is **ufw**; if yours is active
(`sudo ufw status`), allow the port through it:

```console
$ sudo ufw allow 9090/tcp
```

If the firewall is off, no rule is needed — and chapter 1's
rule is the reason to think before reversing that: the firewall
being off is one more argument for keeping Cockpit on a
trusted network only.

## The checks

Three commands, in the order the troubleshooting chapter will
ask for them:

```console
$ systemctl is-enabled cockpit.socket
enabled
$ systemctl is-active cockpit.socket
active
$ ss -tlnp | grep 9090
LISTEN 0  0  *:9090  *:*  users:(("systemd",pid=1,...))
```

Note the listener's owner: **systemd itself** holds port 9090.
That is socket activation working as designed — the web service
is not running yet, and will not be, until someone connects.
Which is the third check:

```console
$ curl -k https://localhost:9090/
```

An answer — the lab's was `HTTP/1.1 200 OK` and a page of HTML
— means the chain works end to end: the connection woke the
socket, the socket started the service, and the service
generated its self-signed certificate on the way up (look in
`/etc/cockpit/ws-certs.d/` afterwards: a `.cert` and a `.key`
file, created at first start, are the evidence). The `-k` flag
tells curl not to verify that certificate; chapter 4 is about
when that is acceptable — on your own machine, at an address
you typed, for a check you planned. It is never acceptable as
a habit elsewhere.

If `curl` is refused while the socket shows active, you are
looking at the troubleshooting chapter's first entry, and the
fix is on the machine, not in the browser. If all three checks pass, you
have a console. The next chapter walks through its front door.
