# The built-in terminal

After this chapter you will know what the Terminal page really
is, when to reach for it instead of a purpose-built page, and
when to close it and use SSH instead.

Open **Terminal** from the menu. What you get is not a
simulation or a restricted command box. It is a real **shell**
— the same command-line interpreter you would get over SSH —
running on the machine, as your user, in your browser tab. Type
`whoami`, `hostname`, `ls`; install something with sudo; edit a
file; the machine cannot tell the difference, because there is
none. Under the covers, Cockpit's session helper starts your
login shell attached to a **pseudo-terminal** (the software
stand-in for a physical terminal that programs expect to talk
to), and your keystrokes and its output travel through the same
encrypted connection as the rest of the console.

That one fact organises everything else about the page.

**Why it exists.** Cockpit's pages are forms over the common
jobs. The terminal is the confession that the uncommon jobs
exist too: the setting with no page, the tool with no button,
the config file that needs one careful line changed. Rather
than pretend otherwise, Cockpit keeps the machine's real
interface one click from every form. When you cannot find a
control, the terminal is not a failure of the console; it is
the console's back wall opening onto the machine itself.

**What it inherits.** Your permissions, exactly. As an ordinary
user, the terminal is an ordinary user's terminal. With
administrative access switched on, `sudo` works as it always
does. There is no special Cockpit privilege and no special
Cockpit restriction: the page cannot protect you from a command
you should not have typed, and it will not stop you from
typing it. The *rm* you run here is as final as any other.

**Working habits that suit it.**

- It behaves like any terminal: it has a size that follows the
  browser window, colours, and full-screen programs (`top`, a
  text editor, an installer) work as they should.
- Copy and paste are your browser's, and occasionally clumsy
  in the way browser terminals are; for pasting a long script,
  write the file properly instead of pouring it into a prompt.
- It ends when you log out or close the session. A program you
  leave running in it is a program attached to that terminal:
  close the tab and it gets the hang-up signal like any
  disconnected terminal. For work that must survive, that is
  what `tmux` and `screen` are for — run one inside this
  terminal, and the work outlives the browser. (This series
  has a guide to tmux.)

**Terminal or SSH?** For a machine you use daily, SSH from your
own terminal program is still the better tool: your keys, your
scrollback, your window habits, no browser in between. The
built-in terminal wins in three situations: you are already in
Cockpit and the job is thirty seconds long; you are on a
machine or network where only the browser (and its port 9090)
can reach the server; or the person you are helping has a
browser and no SSH client set up. Knowing both, and choosing
on purpose, is the whole chapter.
