# Add-on applications

After this chapter you will know where Cockpit's extra pages
come from, meet the three add-ons most worth having, and be
able to install one and find it in the menu — and remove it
again, which is part of the deal.

Chapter 2 mentioned that the console is a set of small web
applications, one folder each under `/usr/share/cockpit/`.
Add-ons are simply more of those: packages you install with
your ordinary package manager, which drop a folder into that
directory and appear in Cockpit's menu the next time you load
the console. There is no app store, no separate account, and no
plugin system to learn. An add-on is a package, the page is
drawn in your browser, and the work is done by the bridge and
the system's services — the same architecture as the built-in
pages, with the same permissions: an add-on can do nothing as
you that you could not do, and with administrative access it
has the same reach as every other page. Install add-ons only
from sources you trust with the machine, which in practice
means your distribution's repositories, or the add-on's own
project if your distribution does not carry it.

## The three worth meeting first

**Cockpit Files** (`cockpit-files`) — a file browser for the
server: navigate folders, upload and download files, edit
small text files, fix permissions, all in the browser. It is
the youngest of the official pages and the one desktop-minded
administrators miss first. Everything it does, your terminal
does too; its value is the picture — seeing a folder's whole
contents, sizes, and dates at a glance before you act. One
honest wrinkle: it is new enough that Ubuntu 24.04 carries it
only in its *backports* repository (newer packages, offered
separately from the standard set), so on that system it needs
backports enabled, where Podman and Machines below install
straight from the standard set.

**Cockpit Podman** (`cockpit-podman`) — manage **containers**
(packaged programs that run in isolation, sharing the host's
kernel) served by Podman, Docker's daemon-free cousin: images,
containers, start and stop, logs, resource graphs. If your
server's real workload is containers, this page turns Cockpit
from a machine console into the workload's console too.
(This series has a whole guide to Podman itself.)

**Cockpit Machines** (`cockpit-machines`) — manage **virtual
machines** (whole simulated computers, each with its own
operating system) run by the machine's libvirt virtualisation:
create a VM, watch its console in the browser, start, stop,
and snapshot it. The heaviest of the three, and the one that
turns a single server into the small fleet of chapter 11 — all
of them reachable, afterwards, as hosts in the same console.

Others exist for particular worlds — an SELinux page on
RHEL-family systems, a report collector (`cockpit-sosreport`)
that bundles a machine's diagnostics for a support ticket, and
community add-ons such as the file-sharing and storage tools
from the 45Drives project. The pattern to remember is the
package, not the catalogue.

## Installing and removing, the whole ceremony

```console
$ sudo apt install cockpit-podman
```

Reload the console in the browser, and **Containers** is in
the menu. Removing the package removes the page. That is
genuinely all of it — and it is worth noticing why: because
the pages hold no state of their own, installing one cannot
strand your data, and removing one cannot break the machine.
Your containers keep existing without their page, exactly as
your services kept existing before Cockpit arrived. Tools
that are easy to leave are tools you can afford to try.
