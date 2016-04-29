Like apps on OS X, Theos is entirely self-contained. It will run from anywhere, as long as the prerequisites are met.

## Officially supported platforms
Theos aims to be able to build on the following platforms.

* OS X Mavericks or newer, with Xcode 5.0 or newer
* Windows 7 or newer, with Cygwin and [CoolStar’s toolchain](http://sharedinstance.net/2013/12/build-on-windows/)
* iOS 5.0 or newer, with [CoolStar’s toolchain](http://moreinfo.thebigboss.org/moreinfo/depiction.php?file=iostoolchainDp)
* Any decently recent Linux distro, using Linux 3.0 or newer, with [CoolStar’s toolchain](https://developer.angelxwind.net/Linux/ios-toolchain_clang%2bllvm%2bld64_latest_linux_x86_64.zip)

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

Don’t forget the `--recursive` flag. The Theos repository contains submodules, and this flag will clone them for you. (If you forget this, change directories to a Theos project and run `make update-theos`.)

In almost all situations, /var and /opt will not be writable. If this is the case, you must use `sudo` on the above command, and then change the owner to yourself:

```console
$ sudo chown $(id -u):$(id -g) theos
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
Theos in its current state is a continuation of the work done by Dustin Howett, with work from Ryan Petrich’s fork included (among others). As such, you can easily “upgrade” from one of these two most commonly used variants of Theos to this one.

Hopefully your copy of Theos was downloaded with Git, and not downloaded as a ZIP file from GitHub (or from DHowett’s Subversion repository). If this is the case, make a backup of everything you’ve changed, delete the Theos directory, and then install Theos from scratch with the directions above.

Simply change the remote repo and pull:

```console
$ git remote set-url origin https://github.com/theos/theos.git
$ git pull origin master
```

Then instruct Git to clone the submodules like so:

```console
$ git submodule update --init --recursive
```

## Moving Theos
Due to a limitation of old versions of Git, repos with submodules can’t easily be moved without breaking Git features.

If you choose to move the location of Theos, you should first run this script to change the submodule paths from absolute to relative:

```bash
#!/bin/bash
# Fix the gitdir value in the .git file of each submodule.
find "$THEOS" -name .git -type f | while read i; do
  old_path="$(grep gitdir "$i" | cut -d: -f2)"

  if [[ "${old_path:0:3}" == "../" ]]; then
    echo "$i is already a relative path"
  else
    new_path="$(realpath --relative-to="$THEOS" "$old_path")"
    echo "gitdir: $new_path" > "$i"
  fi
done

# Fix the worktree value in the config of each submodule.
find "$THEOS"/.git/modules -name config | while read i; do
  old_path="$(git config --file="$i" core.worktree)"

  if [[ "${old_path:0:3}" == "../" ]]; then
    echo "$i is already a relative path"
  else
    new_path="$(realpath --relative-to="$i" "$old_path")"
    git config --file="$i" core.worktree "$new_path"
  fi
done
```