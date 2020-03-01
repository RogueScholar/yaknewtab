# yaknewtab [![GitHub license](https://img.shields.io/github/license/RogueScholar/yaknewtab.svg?logo=GNU&style=for-the-badge)](https://github.com/RogueScholar/yaknewtab/blob/master/LICENSE)

## A shell script for opening new Yakuake tabs/sessions

[![made-with-bash](https://img.shields.io/static/v1.svg?label=Made%20With&message=Bash&color=blue&logo=GNU&style=for-the-badge)](https://www.gnu.org/software/bash/) [![GitHub issues](https://img.shields.io/github/issues/RogueScholar/yaknewtab.svg?logo=GitHub&style=for-the-badge)](https://github.com/RogueScholar/yaknewtab/issues) [![GitHub stars](https://img.shields.io/github/stars/RogueScholar/yaknewtab.svg?logo=GitHub&style=for-the-badge)](https://github.com/RogueScholar/yaknewtab/stargazers) [![GitHub last commit](https://img.shields.io/github/last-commit/RogueScholar/yaknewtab.svg?logo=GitHub&style=for-the-badge)](https://github.com/RogueScholar/yaknewtab/commits/master) ![GitHub repo size](https://img.shields.io/github/repo-size/RogueScholar/yaknewtab.svg?logo=GitHub&style=for-the-badge)

[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2FRogueScholar%2Fyaknewtab.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2FRogueScholar%2Fyaknewtab?ref=badge_shield)

___
_Forked from [yakuake-session](https://github.com/aplatanado/yakuake-session) by [Jesús Torres](https://github.com/aplatanado), © 2010-2018._
___

## What is yaknewtab?

**yaknewtab** is a (mostly) POSIX shell script that greatly simplifies the creation of new tabs/sessions in Yakuake. It uses simple logic to determine the D-Bus CLI tool needed to communicate with the already-running Yakuake process, adding the specifics (supplied as standard POSIX flags and arguments) in the necessary syntax to generate the desired result.

I forked it from the original developer mainly to streamline the approach, more closely following consensus best practices, and ultimately achieving (I believe) greater consistency of operation on a wider range of shells. Like the upstream project the goal is a better integration of Yakuake with the desktop environment (typically KDE Plasma 5) and other resident applications. This is something which has often been constrained by its design as an "always-running" foreground process that remains hidden from view until triggered, insisting on D-Bus as the only method to receive user directives that are not issued through its GUI.

Where on most *nix systems a needed terminal instance can be invoked by simply calling its executable, with Yakuake that would add another running instance, conflicting with the one which was already running and hidden. To successfully accomplish new instance creation with Yakuake (i.e. telling the hidden instance to show itself and add a new tab in the requested working directory) requires use of the D-Bus interface, a task sufficiently esoteric that even many power users struggle to do without hiccups.

For example, thanks to it, Yakuake can replace Konsole in "Open terminal here" action in Dolphin or we can setup a menu similar to Konsole Profiles widget but using Yakuake instead of Konsole.

## What is Yakuake?

A terminal emulator developed and distributed by KDE as part of the Plasma desktop environment. Visually, it seeks to hearken to the celebrated console panel from the 90s computer game, Quake: sliding into and out of view from the (top) edge of the screen upon the press of a hotkey.

The name is an exquisite example of over-the-top open source project naming irony. Technically a portmanteau crossed with a "backronym," it's derived from the motto "Yet Another Kuake." In keeping with KDE project tradition, phonemes that can be represented with the letter 'k' are styled as such, explaining the odd spelling.

### _Links to more information about Yakuake_

* [Project Home Page](https://kde.org/applications/system/yakuake/)
* [Browsable Source Code Repository](https://cgit.kde.org/yakuake.git/) (or clone via [git](git://anongit.kde.org/yakuake.git)/[https](https://anongit.kde.org/yakuake.git))
* [KDE Bugtracking System](https://bugs.kde.org/buglist.cgi?component=general&list_id=1612098&product=yakuake&resolution=---)
* [OpenDesktop.org - Yakuake](https://www.linux-apps.com/content/show.php?content=29153)
* [KDE UserBase Wiki](https://userbase.kde.org/Yakuake)
* [Freenode Web Chat: #yakuake](http://webchat.freenode.net/?randomnick=1&channels=%23yakuake&prompt=1&uio=MTY9dHJ1ZSY5PXRydWUmMTI9dHJ1ZQ6c) ([irc:// link](irc://irc.freenode.org/#yakuake))

## Bugs & known issues

* Requires wmctrl to change focus if yakuake is already open. Get it from the repo e.g. `apt install wmctrl`

* Fish is not a POSIX compliant shell, so yaknewtab detects if it is used for the invocation and adjusts its operations accordingly. If the auto-detection doesn't work properly, two solutions exist:
  * Issuing the command `export FISH_SHELL=0` temporarily disables the Fish shell adaptations, while `export FISH_SHELL=1` temporarily enables them

  * For a permanent change, use the following command (provided that you placed it in a location in your $PATH) to open the script file in the default editor and change line 14 accordingly: `FISH_SHELL=1` to enable the Fish shell changes or `FISH_SHELL=0` to disable them.

      ```bash
      command -v yaknewtab | xargs sudoedit -e
      ```

## Installation

Clone this repository and navigate into the new repository directory.

```bash
git clone https://github.com/RogueScholar/yaknewtab.git && cd yaknewtab
```

Copy the yaknewtab script to `~/bin`, `/usr/local/bin` or some other directory in your `$PATH` variable so it can be executed by just its name.

If the file is in any other location you will be forced to type the relative or full path of the script every time you wish to use it.

```bash
# Examine your $PATH environment variable to aid in selecting an appropriate folder
# (Shown here with example output)

$ echo $PATH
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:$HOME/bin"

# Copy the script to the folder of your choice and ensure
# it has the right ownership and permissions

# Installation system-wide (for all users) in /usr/local/bin
$ sudo install -Dp --mode=755 --owner=root --group=root yaknewtab /usr/local/bin/yaknewtab

# Installation for the current user only in $HOME/bin
$ install -Dp --mode=755 yaknewtab $HOME/bin/yaknewtab
```

## Usage

To invoke yaknewtab:

`yaknewtab`

Without arguments, yaknewtab creates a new session in the currently running Yakuake terminal emulator.

The option `-e` allows to indicate a command to execute in the new session.

```bash
yaknewtab -e ls
```

The argument `-t` sets the title for the new tab.

```bash
yaknewtab -t "Title"
```

The session working directory is changed to the directory where yaknewtab
was called, before execute the command. If we want to launch the command from
user's home directory then we would use the option `-h`.

```bash
yaknewtab -h -e ls
```

If the command requires some arguments, they are taken from non-option
arguments passed to yaknewtab. That means that if some arguments for the
command begin with `-`, they must passed to yaknewtab after `--`.

```bash
yaknewtab -h -e ssh -- -X user@example.com
```

Another useful option is `--workdir`. It allows to change the working directory
before execute the command.

```bash
yaknewtab --workdir /tmp -e ls
```

yaknewtab has many other options that were shown in help.

```bash
$ yaknewtab --help

  Usage: yaknewtab [options] [args]

    Options:
      -q                      Keep the Yakuake window hidden
      -e <cmd>                Command(s) to execute; this flag will catch all subsequent arguments, so must be called last
      -t <title>              Set <title> as the title of the new tab
      -p <property=value>     Change the value of a profile property (only for KDE 4)
      -h, --homedir           Open a new tab with '$HOME' as the working directory
      -w, --workdir <dir>     Open a new tab with <dir> as the working directory
      --hold, --noclose       Do not close the session automatically when the command ends
      --fish | --nofish       Manually enable or disable the fish shell autocompletion support
      --debug                 Redirect script debugging output to the console
      --help                  Print this message

    Arguments:
      args                    Arguments passed to command from '-e' flag
```

## Action in Dolphin

Dolphin provides the action "Open terminal here" that opens a Konsole terminal
emulator in the specified folder. This behavior can be changed to use Yakuake
instead of Konsole coping `konsolehere.desktop` into KDE Service Menus.

```bash
# KDE 4
cp ServiceMenus/konsolehere.desktop ~/.kde/share/kde4/services/ServiceMenus/
```

```
# KDE 5: for the current user only.
cp ServiceMenus/konsolehere.desktop ~/.local/share/kservices5/ServiceMenus/
```

or

```
# KDE 5: system-wide
cp ServiceMenus/konsolehere.desktop /usr/share/kservices5/ServiceMenus/
```

If we do not want to change the behavior of "Open terminal here", then copy
`yaknewtab-here.desktop` instead to add the new action "Open yakuake here" to
Dolphin.

```bash
# KDE 4
cp ServiceMenus/yaknewtab-here.desktop ~/.kde/share/kde4/services/ServiceMenus/
# KDE 5
cp ServiceMenus/yaknewtab-here.desktop ~/.local/share/kservices5/ServiceMenus/
```

or

```
# KDE 5: system-wide
cp ServiceMenus/yakuakehere.desktop /usr/share/kservices5/ServiceMenus/
```

## Quick Access Menu

Konsole Profiles is a Plasma widget that allows to open a new terminal window, configured according to a select profile, and automatically execute a command in it. We can get something similar but for Yakuake using the QuickAccess widget.

We only have to make a directory and setup a QuickAccess widget instance to use it as origin (I also like to disable the browsing). Then we add some "Link to Application" to that directory, such that each one use yaknewtab to create a new Yakuake session and to execute the command that we want.

The file `examples/yaknewtab-ssh.desktop` contains an example that launch a ssh client in a new Yakuake session.

## Contributing and licensing

[![Most recent contributor](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/images/0)](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/links/0)[![2nd most recent contributor](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/images/1)](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/links/1)[![Third most recent contributor](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/images/2)](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/links/2)[![Fourth most recent contributor](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/images/3)](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/links/3)[![Fifth most recent contributor](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/images/4)](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/links/4)[![Sixth most recent contributor](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/images/5)](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/links/5)[![Seventh most recent contributor](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/images/6)](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/links/6)[![Sourcerer.io Contributors Widget About Page](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/images/7)](https://sourcerer.io/fame/RogueScholar/RogueScholar/yaknewtab/links/7)

Contributors need to sign the [Contributor License Agreement](http://contributoragreements.org/ca-cla-chooser/?beneficiary-name=Peter+J.+Mello&project-name=yaknewtab&project-website=https%3A%2F%2Fgithub.com%2FRogueScholar%2Fyaknewtab&project-email=admin%40petermello.net&process-url=https%3A%2F%2Fgithub.com%2FRogueScholar%2Fyaknewtab&project-jurisdiction=United+States+of+America&agreement-exclusivity=exclusive&fsfe-compliance=&fsfe-fla=&outbound-option=fsfe&outboundlist=GPL-3.0&outboundlist-custom=&medialist=CC-BY-NC-SA-4.0&patent-option=Traditional&your-date=&your-name=&your-title=&your-address=&your-patents=&pos=apply&action=) before their pull requests will be reviewed.

___

[![SPDX Logo](https://user-images.githubusercontent.com/15098724/57586996-dc44ae00-74b2-11e9-92a4-8ad1b6d81d0a.png)](https://spdx.org/) [SPDX-License-Identifier: GPL-3.0-or-later](https://spdx.org/licenses/GPL-3.0-or-later.html)

[![GNU GPL-3.0 License Logo](https://user-images.githubusercontent.com/15098724/57586281-8bc85300-74a8-11e9-9ae6-0fe7b191c46f.png)](https://www.gnu.org/licenses/gpl.html) [![GNU FDL-1.3 License Logo](https://user-images.githubusercontent.com/15098724/57586323-23c63c80-74a9-11e9-8e02-700e241c1710.png)](https://www.gnu.org/licenses/fdl.html)

## Contact

[![Keybase PGP](https://img.shields.io/keybase/pgp/rscholar.svg?label=Keybase.io&logo=Keybase&logoColor=white&style=for-the-badge)](https://keybase.io/rscholar) [![Twitter Follow](https://img.shields.io/twitter/follow/SingularErgoSum.svg?color=orange&label=Follow%20%40SingularErgoSum&logo=Twitter&style=for-the-badge)](https://twitter.com/SingularErgoSum)

![Creative Commons ShareAlike Symbol-20px](https://user-images.githubusercontent.com/15098724/56478451-5958a680-6464-11e9-944a-f4c744e70f26.png) Copyleft: All rights reversed. 2019 [Peter J. Mello](https://github.com/RogueScholar)
