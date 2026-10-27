# Troubleshooting

After this chapter you will be able to take the failures
Cockpit actually has — in the order they actually occur — and
match each to the one check that settles it. The method
throughout is chapter 2's: name the part, then test the part.

**The address does not answer at all.**
Nothing at `https://machine:9090` — a timeout or a refusal.
Work from the machine outward. On the machine:
`systemctl status cockpit.socket` — it should be enabled and
*active (listening)*; if it is not, `sudo systemctl enable
--now cockpit.socket` is the fix, and chapter 3 is where the
guide set it. Then `ss -tlnp | grep 9090` to prove something
holds the port. Then the network: are you addressing the
machine you think you are, on a network it is actually on?
Finally the firewall — a machine running ufw or firewalld with
no rule for 9090/tcp will look exactly like this from outside
while being perfectly healthy inside: `curl -k
https://localhost:9090/` *on the machine* is the test that
separates "Cockpit is down" from "the door is shut".

**The browser warns about the certificate.**
On a fresh install, the one-time self-signed warning is
expected — chapter 4 covers judging it (your machine, your
address, proceed once). A warning that *returns* after you
accepted it, or that appears on a machine you did not just
install, is different: the certificate changed, or you are not
talking to the machine you think you are. Stop and find out
which before proceeding; compare the certificate's
fingerprints with the files in `/etc/cockpit/ws-certs.d/` if
you must. The warning is the system working; treat it that way.

**Login is refused.**
Cockpit uses the machine's accounts, so login problems are
account problems: wrong password, a locked account, or an
account that does not exist on *this* machine (mind chapter
11's host switcher — check which host you are logging in to).
After several failures PAM may impose a delay (a deliberate
pause after repeated wrong passwords); wait it out rather
than hammering. And root login specifically may be refused by
policy — `root` is commonly blocked from direct web login;
log in as yourself and use administrative access, as chapter 4
prescribed. The journal holds the evidence: `journalctl -u
cockpit` around the failed attempt says which check refused
you.

**You are in, but a page is empty or missing.**
The page is a window; the room behind it is a system service.
Storage empty → is `udisks2` running, and is `cockpit-storaged`
installed? Networking blank → is the machine's networking run
by NetworkManager at all? Updates missing → `cockpit-packagekit`
installed, PackageKit running? Check the page's own package
and its backing service (`systemctl status udisks2
NetworkManager packagekit`) before suspecting Cockpit itself.
A page that vanished from the menu entirely had its package
removed — the menu is the directory `/usr/share/cockpit/`,
drawn live.

**Buttons are greyed out; everything is read-only.**
You are logged in as a user without administrative rights, or
you have not unlocked them. Chapter 4's **Administrative
access** button, your password again, and the controls light
up — provided your account may use sudo at all. If the button
itself is absent, your account is not in the administrators
group, and only an existing administrator can put it there
(chapter 8). This is Cockpit's most-reported "bug" and it is
the permission system working perfectly.

**It worked yesterday.**
Something changed; the journal remembers what. `journalctl -u
cockpit.socket -u cockpit` shows the web service's own recent
life, including failures to start after a bad certificate file
was dropped into `ws-certs.d/` (a malformed certificate is the
classic self-inflicted wound here — remove the newest file and
the socket recovers). Updates that replaced a page package can
also rearrange a menu. When in doubt, the loop that solves
most of this chapter is the one from chapter 5: read the log,
then touch the thing.
