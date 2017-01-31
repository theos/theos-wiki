Like apps on macOS, Theos is entirely self-contained. It will run from anywhere, as long as the prerequisites are met.

## Officially supported platforms
Theos aims to be able to work on, and build for, the following platforms.

| Platform | Minimum OS version | Prerequisites | Toolchain | Targets supported
|----------|--------------------|---------------|-----------|-------------------|
| **macOS** | Mavericks (10.9) | — | Xcode 5.0 or newer. Xcode 4.4 supported, but only when building for ARMv6 (1st/2nd generation iPhone/iPod touch). | macOS, iOS, watchOS, tvOS<sup>1</sup> |
| **iOS** | 5.0 | Jailbroken | [CoolStar’s toolchain](http://cydia.saurik.com/package/org.coolstar.iostoolchain/) (package on BigBoss repo) | iOS |
| **Windows** | 7 | [Cygwin](https://cygwin.com/) with OpenSSH and make | [CoolStar’s toolchain](http://sharedinstance.net/2013/12/build-on-windows/) (tutorial) | Windows (Cygwin), iOS |
| **Linux** | Linux kernel 3.0 | [build-essential](https://packages.debian.org/sid/build-essential) or equivalent | [CoolStar’s toolchain](https://developer.angelxwind.net/Linux/ios-toolchain_clang%2bllvm%2bld64_latest_linux_x86_64.zip) (direct ZIP download) | Linux, iOS |

<sup><sup>1</sup> Supports jailbroken devices and simulators, where applicable.</sup>

Other platforms may or may not work, but be aware that they are unsupported. Theos may work on them now, but it may not in future. If you think we should officially support a platform not listed here, [get in touch](https://github.com/theos/theos/issues/new).

## Prerequisites
* Git (included with Xcode)
* Toolchains and SDKs for the platforms you intend to build for

On macOS, Xcode is mandatory. The Command Line Tools package isn’t sufficient for Theos to work.

If you're building for iOS, you should also have dpkg and [ldid](http://iphonedevwiki.net/index.php/Ldid) installed. On macOS, you can do so via Homebrew:

```console
$ brew install dpkg ldid
$ brew install --HEAD hbang/repo/deviceconsole  # (not required, but very useful)
```

On iOS, install:

* ldid (Link Identity Editor), available from Telesphoro
* Perl, available from [coolstar's public repository](https://coolstar.org/publicrepo/)
* CA Certs, available from [BigBoss](http://cydia.saurik.com/package/org.thebigboss.cacerts/)

## Installation
Decide where you want to install Theos. The most common places are `~/theos`, `/opt/theos`, or `/var/theos`.

```console
$ git clone --recursive https://github.com/theos/theos.git
```

Don’t forget the `--recursive` flag. The Theos repository contains submodules, and this flag will clone them for you. (If you forget this, change directories to a Theos project and run `make update-theos`.)

In almost all situations, /var and /opt will not be writable. If this is the case, you must use `sudo` on the above command, and then change the owner to yourself:

```console
$ sudo chown -R $(id -u):$(id -g) theos
```

While it is possible to download Theos using the “Download ZIP” button on GitHub, this is discouraged as it will make it hard to update Theos in future.

You must also set the `$THEOS` variable in your environment, and export it so `make` will see its value when you run it. Edit `~/.bash_profile` (or equivalent for your shell of choice) and add this, replacing the value with the full path to where Theos is installed:

```bash
export THEOS=/absolute/path/to/theos
```

In the same file, add the binaries path to `$PATH` so you don't need to preface commands with `$THEOS/bin/` every time:

```bash
export PATH=$THEOS/bin:$PATH
```

In order to use `make troubleshoot`, you need to install Ghostbin’s [ghost.sh](https://ghostbin.com/ghost.sh) script.

```console
$ curl https://ghostbin.com/ghost.sh -o $THEOS/bin/ghost
$ chmod +x $THEOS/bin/ghost
```

Further setup may be required, depending on the platforms you will be building for. Visit iPhone Dev Wiki’s [Theos/Setup](http://iphonedevwiki.net/index.php/Theos/Setup) page for more details.

## Updating
Theos utilises a [rolling release](https://en.wikipedia.org/wiki/Rolling_release) model, meaning the latest commit to the Git repo is the latest version of Theos available. Occasionally, you should update Theos. This can be done simply by switching to a directory containing a Theos makefile and then running:

```console
$ make update-theos
```

If you experience problems, updating Theos is the first thing you should do. This makes it a lot easier to track down the problem if you ask someone for help.

If you see the following when running the command:

```
make: *** No rule to make target `update-theos'.  Stop.
```

…then you are either not currently in a project directory, or are using a version of Theos older than this feature. See the following section.

## Switching from DHowett’s or rpetrich’s Theos
Refer to [[Upgrading from legacy Theos]].

## Moving Theos
Due to a limitation of old versions of Git, repos with submodules can’t easily be moved without breaking Git features.

If you choose to move the location of Theos, you should first run this script to change the submodule paths from absolute to relative:

```bash
#!/bin/bash
# Fix the gitdir value in the .git file of each submodule.
find "$THEOS" -name .git -type f | while read i; do
  old_path="$(grep gitdir "$i" | cut -d: -f2)"

  if [[ "${old_path:1:3}" == "../" ]]; then
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

Alternatively, just delete the directory (make sure to grab a copy of anything you put there that might be valuable!) and [[clone it from scratch|Installation#installation]].