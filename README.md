XSuspender
==========
[![Build Status](https://travis-ci.org/kernc/xsuspender.svg?branch=master)](https://travis-ci.org/kernc/xsuspender)

Automatically suspend inactive X11 applications.

Find a better maintained description here: https://kernc.github.io/xsuspender/

When an application window loses focus, XSuspender tries to match it to
one of the rules in its configuration. If a match is found, the
application is sent a SIGSTOP signal (preventing the process from obtaining
further CPU time). Upon windows regaining focus, the process is seamlessly
continued where it had left off.

#### Advantages

* **Reduce battery use (increase battery run-time).**  
  Make your laptop run on battery for as long as your mobile phone does,
  using roughly the same technique.
* **Reduce interaction latency on low-end CPUs.**  
  With fewer clients requesting processing power, there's more of it to go
  around where it's needed.
* **Reduce CPU fan noise.**  
  Save the tinnitus for old age.
* **Avoid apps plotting stuff behind your back.**  
  That Kali you're running in a VM is perfectly fine, but god
  only knows what Microsoft Windos is doing.
* **Suspend processes using well-known Unix signals SIGSTOP & SIGCONT ...**  
  ... or custom shell scripts. Decades of portable operating systems
  engineering at its finest.
* **Preconfigured for recent versions of popular software.**  
  Chromium, Firefox, JetBrains IDEs, qBittorrent, VirtualBox ...

#### Quirks

* Quirky. See [Notes] below.
* May prevent suspended windows from redrawing until re-gaining focus.
* May make your web downloads stall and your in-browser media
  playback stop if you configure it thus.
* Prevents pasting from clipboard while the selection source process
  is suspended
  ([explanation](https://unix.stackexchange.com/questions/316715/xclip-works-differently-in-interactive-and-non-interactive-shells/316890#316890)).
* Relies on windows having their `_NET_WM_PID` hint set correctly.
* Won't work in remote X sessions.
* Won't work with Wayland.
 

Installation
------------

#### From Source

```bash
# Install build dependencies, namely GLib and Libwnck
sudo apt install libglib2.0-dev \
                 libwnck-3-dev  \
                 make cmake gcc pkg-config
```

```bash
# Fetch a copy of the source code
git clone https://github.com/kernc/xsuspender
cd xsuspender

# Move to build directory for an out-of-tree build
cd build

# Configure and make
cmake -DCMAKE_INSTALL_PREFIX=/usr/local ..
make
make test

# Install within chosen prefix
sudo make install
```

Usage
-----
For brief usage instructions, run:

```bash
xsuspender --help
```

#### Configuration debugging

To have it print verbose debug messages about what it is doing, run the
program with environmental variable `G_MESSAGES_DEBUG=all` set:

    G_MESSAGES_DEBUG=all xsuspender
    
This is _strongly recommended_ to confirm your customized configuration
rules indeed work as you expect.

If xsuspender is auto run by your X session manager, you might find clues
to its unexpected behavior in _~/.xsession-errors_.

Notes
-----
[Notes]: #notes

* Processes that take a long time to shut down after their window already
  disappears may be stopped in the middle of their termination routines.
  Avoid with reasonably generous `suspend_delay`.
* Windows that minimize to system tray need to be awaken frequently to
  respond to click events in a seamless manner.
* Don't configure xsuspender for software you want to keep continuously alive
  in the background, such as music players, daemons, IM clients ... If you
  frequently stream music from YouTube, you might give [Musictube] a try.
  
[Musictube]: https://flavio.tordini.org/musictube
