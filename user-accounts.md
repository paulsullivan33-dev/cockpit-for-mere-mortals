# User accounts

After this chapter you will be able to create an account in
Cockpit, give it the right amount of power, set or reset its
password, and retire an account without breaking the things it
owned.

Open **Accounts**. The page lists the machine's human users —
the accounts a person can log in with. (The machine also has
many *system* accounts, one per service, that no person logs in
as; Cockpit keeps the list focused on the human kind.) Each
entry shows the username, the person's real name as recorded
on the system, and whether the account is locked.

Everything in this chapter needs administrative access; account
management is the machine's keys, and chapter 4's rule applies:
you are doing sudo's work, in the open, logged as you.

## Creating an account

Click **Create new account** and fill in the form:

- **Full name** — the person's name, stored on the account and
  shown in listings.
- **User name** — the login name: short, lowercase, no spaces.
  It also names their home directory (`/home/name`) and can
  never be changed casually later. Choose like it matters,
  because it does.
- **Password** — set at creation, or leave the account to be
  given one at first login. The same rules as any password on a
  machine that matters: long, unique, never reused from
  elsewhere. It is stored by the system as a one-way hash (a
  scrambled fingerprint that can be checked but not reversed)
  in the protected shadow file — Cockpit never keeps a copy.
- **Server administrator** — a checkbox that grants the account
  sudo rights by putting it in the machine's administrators
  group (`sudo` on Ubuntu and Debian, `wheel` on Fedora and
  RHEL). This is the single most consequential box on the form.
  Tick it for people who should be able to run the whole
  machine, which is a shorter list than people who should be
  able to log in to it.

## Looking after accounts

Open an account for its own page:

- **Set password** — reset a forgotten one. The account's owner
  will be made to choose a new one at next login if you say so.
- **Lock account** — refuse logins without deleting anything.
  Locking is the reversible step: use it the day someone
  leaves, leaves, and think about deletion later.
- **Delete** — removes the account, with a choice about its
  home directory and files: keep them (they become files owned
  by a number, waiting for a decision) or delete them too.
  Deleting the account is final; deleting the files more so.
- **Authorized SSH keys** — the account's public SSH keys,
  listed and editable. A key listed here lets its holder log in
  without a password. Adding one for someone is handing them a
  house key: do it on purpose, and remove keys that no longer
  belong to anyone current. (SSH itself is its own guide in
  this series; this page is only the key drawer.)
- **Session and account history** — recent logins and the
  account's activity, which is the first place to look when
  "something happened to my account" is the report.

One habit this page makes easy, and worth making a rule:
**one account per person, never shared.** Because Cockpit logs
in as you and acts as you, a shared account makes the machine's
own records useless — every action belongs to everyone. The
Accounts page takes thirty seconds to give someone their own
name; take the thirty seconds.
