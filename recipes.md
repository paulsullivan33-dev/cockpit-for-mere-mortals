# Recipes

After this chapter you will have a small shelf of complete,
named procedures — the jobs people actually open Cockpit for —
each with its checks, so a job is done when its check passes,
not when you feel finished.

**1. The Monday health check (five minutes, one machine).**
Open the Overview and read the complaint cards: failed
services, pending updates. Click into **Services** and filter
to failed — there should be none, and if there is one, its own
page holds the reason. Open **Logs**, set the priority to
*Errors and above* and the range to the last 7 days, and read
what is there; a short, boring list is a healthy machine. Open
**Storage** and confirm no filesystem is above roughly ninety
percent full. You are done when all four checks are read, not
when all four are green — a check you looked at is worth ten
you assumed.

**2. A service keeps dying.**
Open the service's page and read the log panel *first*: the
last lines before the failure are the diagnosis more often than
not (a missing file, a refused port, a full disk — chapter 5's
and 6's pages cross-check the last two). Fix the cause, then
**Restart**, then watch the service's state and its log for a
full minute before declaring victory. Finally check the boot
setting: a service that is running but *disabled* will perform
this same death at the next reboot.

**3. Give a new colleague an account, and nothing more.**
Follow chapter 8: create the account, a real name, a password
they will change, and **leave the administrator box unticked**.
Log in as them in a private browser window and confirm the
console is read-only — that view is exactly the power you meant
to give. Tick the administrator box later, on purpose, the day
the job needs it; powers granted in advance have a way of
becoming powers nobody remembers granting.

**4. The disk is full and nobody knows with what.**
Storage page first: which filesystem is full. Then the
terminal page and `du -h --max-depth=1 /` (or the path in
question), descending into the largest folder until the
culprit is a file with a name — usually a log that never
rotated, a cache, or backups landing on the wrong disk. Fix
the cause (delete, move, or configure the rotation) and
re-check the filesystem's free space on the Storage page
before closing the tab.

**5. Apply this month's updates like you mean it.**
Chapter 10: read the list before installing — know whether
the kernel is in it. Install all. If the page says a restart
is required, schedule it rather than admiring the warning;
then, after the reboot, return to the Overview and confirm
the complaint card is gone and **Services** shows nothing
failed. An update round is finished when the machine is
running the new code, not when the packages are downloaded.

**6. Add the new server to the fleet console.**
Chapter 11: on the new machine, install only the bridge and
the pages you use — no web service, no port 9090. From your
existing console's host switcher, add it by name, confirm its
host-key fingerprint against the machine itself, and log in.
Then do recipe 1 on it immediately: a machine you have just
met deserves a health check before it deserves trust.

**7. Something on the web console itself feels wrong.**
Do not reinstall. Name the part, per chapter 2: login page
silent → check `cockpit.socket`; a page empty → check the
service behind it (`udisks2` for Storage, `NetworkManager`
for Networking); buttons grey → administrative access, not
breakage. The troubleshooting chapter is organised in the
same order, and it is faster than folklore.
