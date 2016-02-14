Like apps on OS X, Theos is entirely self-contained. It will run from anywhere, as long as the prerequisites are met.

## Officially supported platforms
Theos aims to be able to build on the following platforms.

* OS X Mavericks or newer, with Xcode 5.0 or newer
* Windows 7 or newer, with Cygwin and [CoolStar’s toolchain](http://sharedinstance.net/2013/12/build-on-windows/)
* iOS 5.0 or newer, with [CoolStar’s toolchain](http://moreinfo.thebigboss.org/moreinfo/depiction.php?file=iostoolchainDp)
* Any decently recent Linux distro, using Linux 3.0 or newer

Other platforms may or may not work, but be aware that they are unsupported.

## Prerequisites
* Git (included with Xcode)
* Toolchains and SDKs for the platforms you intend to build for

On OS X, Xcode is mandatory. The Command Line Tools package isn’t sufficient for Theos to work.

If you're building for iOS, you should also have dpkg and [ldid](http://iphonedevwiki.net/index.php/Ldid) installed. On OS X, you can do so via Homebrew:

```console
$ brew install dpkg ldid
$ brew install --HEAD hbang/repo/deviceconsole  # (not required, but very useful)
```

In order to use `make troubleshoot`, you need to install Ghostbin's [ghost.sh](https://ghostbin.com/ghost.sh) script.

```console
$ curl https://ghostbin.com/ghost.sh -o /usr/local/bin/ghost
$ chmod +x /usr/local/bin/ghost
```

Alternatively, you could install it to `$THEOS/bin/ghost`, but it's useful enough that you probably want it in /usr/local/bin anyway!

## Caveat
Please note that, by default, `theos` symlinks are not made by this Theos fork in new projects created by NIC. You must set and export the `$THEOS` variable in your environment. See [[the FAQ|FAQ#wheres-the-theos-symlink]] for details.

## Installation
Decide where you want to install Theos. The most common places are `~/theos`, `/opt/theos`, or `/var/theos`.

```console
$ git clone --recursive https://github.com/theos/theos.git
```

Don’t forget the `--recursive` flag. The Theos repository contains submodules, and this flag will clone them for you.

In almost all situations, /var and /opt will not be writable. If this is the case, it is advised that you do not add `sudo` onto the `git clone` command unless you have a particular reason to. Clone it to a location you have write access to such as your home directory, then move it as root:

```console
$ sudo mv theos /opt/theos
```

While it is possible to download Theos using the “Download ZIP” button on GitHub, this is discouraged as it will make it hard to update Theos in future.

Further setup may be required, depending on the platforms you will be building for. Visit iPhone Dev Wiki’s [Theos/Setup](http://iphonedevwiki.net/index.php/Theos/Setup) page for more details.

## Updating
Theos utilises a [rolling release](https://en.wikipedia.org/wiki/Rolling_release) model, meaning the latest commit to the Git repo is the latest version of Theos available. Occasionally, you should update Theos. This can be done simply by switching to a directory containing a Theos makefile and then running:

```console
$ make update-theos
```

If you experience problems, updating Theos is the first thing you should do. This makes it a lot easier to track down the problem if you ask someone for help.

## Switching from DHowett’s or rpetrich’s Theos
This fork of Theos is a continuation of the work done by Dustin Howett, with work from Ryan Petrich’s fork included (among others). As such, you can easily “upgrade” from one of these two most commonly used variants of Theos to this one.

Hopefully your copy of Theos was downloaded with Git, and not downloaded as a ZIP file from GitHub (or from DHowett’s Subversion repository). If this is the case, make a backup of everything you’ve changed, delete the Theos directory, and then install Theos from scratch with the directions above.

First – please be sure to move the `include` directory in your existing Theos to elsewhere for now. Since this directory is a Git submodule pointing at the [hbang/headers](https://github.com/hbang/headers) repo, Git will complain about a merge conflict. You can move your headers back there if you want afterwards.

Now, simply change the remote repo and pull:

```console
$ git remote set-url origin https://github.com/theos/theos.git
$ git pull origin master
```

Then grab the `include` submodule like so:

```console
$ git submodule update --init --remote
```