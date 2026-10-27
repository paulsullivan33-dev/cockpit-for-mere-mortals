# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab administered only itself.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 15 names what the tool
   cannot do: one machine at a time changed by hand, no
   watching or alerting while you are away, pages only where a
   system service backs them, the console's power as its own
   risk, and the machine that will not boot at all.
8. **The ethics rule is in the text, early.** Chapter 1 and the
   README state the rule plainly — *your machines or ones you
   administer, on networks you trust, with real passwords and
   current updates* — before any procedure depends on it, and
   later chapters point back to it where the tooling would
   make crossing the line easy.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, 7.7 GB Ubuntu 24.04 sandbox with
systemd running as PID 1. Cockpit **314-1** was installed from
Ubuntu's own repositories, exactly as chapter 3 describes, and
the transcripts live in `cockpit-lab/transcripts/`.

Run for real, with transcripts as evidence:

- The environment: Ubuntu 24.04.5, systemd 255 as PID 1, and
  Cockpit absent before the lab began (t00).
- The install itself: `apt-get install cockpit` bringing in
  cockpit-ws, cockpit-bridge, cockpit-system, cockpit-storaged,
  cockpit-networkmanager and cockpit-packagekit, all at 314-1
  (t02). The sandbox's package system was busy with its own
  maintenance when the lab began; the transcript keeps the
  lock-refused attempts as well as the success, which waited
  for the lock rather than forcing anything.
- The on-disk inventory the chapters describe: the package
  list, the page folders under `/usr/share/cockpit/` (shell,
  systemd, storaged, networkmanager, users, packagekit), the
  programs in `/usr/lib/cockpit/` including cockpit-ws,
  cockpit-session and cockpit-ssh (t03).
- The socket: `cockpit.socket` enabled by the package; in this
  sandbox it also had to be started once by hand, the sandbox
  not booting to a full multi-user target — on a normally
  booted machine chapter 3's `enable --now` covers both.
  Started, it shows *active (listening)* on `[::]:9090`, and
  `ss` shows the listener owned by systemd, PID 1 (t04).
- Socket activation, end to end: `cockpit.service` *inactive*
  before any connection; then `curl -k https://localhost:9090/`
  answered `HTTP/1.1 200 OK` with the login application's HTML
  (the page's own title is set by its scripts; the served page
  identifies itself as Cockpit's login, and the shell that
  curl receives is titled "Loading..."), and the service
  *active* afterwards, started by the socket (t05, t06).
- The self-signed certificate, generated on that first start:
  `0-self-signed.cert` and `0-self-signed.key` appeared in
  `/etc/cockpit/ws-certs.d/`, and openssl reads the certificate
  back with subject equal to issuer — self-signed, as chapter
  4 describes — with its SHA-256 fingerprint (t05). The
  journal shows `cockpit-certificate-ensure` doing the work as
  the service started (t06).
- The add-on packages of chapter 12 exist where the chapter
  says they do: cockpit-podman and cockpit-machines as
  candidates in Ubuntu 24.04's standard repositories,
  cockpit-files in its backports (t06).

Documented only — described from the project's documentation,
not run, and the text does not claim otherwise:

- The interactive web console: logging in with a real user's
  password and clicking through the Services, Storage,
  Networking, Accounts, Terminal and Updates pages. The lab
  verified the login page answers and the service starts; it
  did not drive the interface, and no chapter claims a click
  the lab did not make.
- Everything in chapters 5 to 10 beyond the pages' existence:
  restarting a service, formatting anything, editing an
  interface, creating an account, applying updates. The lab
  was not about to practise destructive operations for
  evidence it could describe honestly instead.
- Adding a second machine (chapter 11): the lab has one
  machine. The mechanism — the bridge over SSH, the small
  package set on the far side — is stated from the
  documentation and from the verified presence of
  cockpit-ssh and the bridge on the near side.
- Installing or running the add-ons themselves (chapter 12):
  their repository availability was verified; their interfaces
  are documented, not demonstrated.
- The dnf and firewalld commands in chapter 3, and the Debian
  backports advice: the lab is an Ubuntu machine, and those
  paths are the project's documented ones, labelled as such
  in the text.

Fixtures are this guide's own and deliberately generic: one
small sandbox machine administering itself, an example address
in the private 192.168 range used only to show the shape of an
address, and no accounts but the sandbox's own. No example is
modelled on any real person, network, service or organisation.
