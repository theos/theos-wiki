## Officially supported platforms
Theos aims to be able to work on, and build for, the following platforms.

| Platform | Minimum OS version | Prerequisites | Targets supported
|----------|--------------------|---------------|-------------------|
| **macOS** | Mavericks (10.9) | Xcode 5.0 or newer. Xcode 4.4 supported, but only when building for ARMv6 (1st/2nd generation iPhone/iPod touch). | macOS, iOS, watchOS, tvOS<sup>1</sup> |
| **iOS** | 5.0 | Jailbroken device; [CoolStar’s repository](https://coolstar.org/publicrepo/) (and [Sam Bingner’s repository](http://repo.bingner.com/) if on iOS 11) installed; [Theos Dependencies](http://moreinfo.thebigboss.org/moreinfo/depiction.php?file=theosdependenciesDp) (package on BigBoss repo) | iOS |
| **Linux** | Linux kernel 3.16 | build-essential (or equivalent for your distro), fakeroot, git, perl; [CoolStar’s toolchain](https://developer.angelxwind.net/Linux/ios-toolchain_clang%2bllvm%2bld64_latest_linux_x86_64.zip) (direct ZIP download) | Linux, iOS |
| [**Windows Subsystem for Linux**](https://docs.microsoft.com/windows/wsl) | Windows 10 build 14393 | (Same as Linux) | (Same as Linux) |
| **Windows: [Cygwin](https://cygwin.com/)**<sup>2</sup> | Windows 7 | curl, git, make, openssh, perl; [CoolStar’s toolchain](http://sharedinstance.net/2013/12/build-on-windows/) (tutorial)| Windows (Cygwin), iOS |

<sup><sup>1</sup> Supports jailbroken devices and simulators, where applicable.  
<sup>2</sup> Please consider using Windows Subsystem for Linux instead if possible. Cygwin works but is quite limited.</sup>  
Other platforms (or versions older than listed above) may work, but be aware that they are unsupported. Theos may work on them now, but it may not in future. If you think we should officially support a platform not listed here, [get in touch](https://github.com/theos/theos/issues/new).

## Prerequisites
* Packages listed for your OS above
* Toolchain for the platform(s) you intend to build for

On macOS, [Xcode](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12) is mandatory. The Command Line Tools package isn’t sufficient for Theos to work. Xcode includes toolchains for all Apple platforms. On any other platform, you will need to download the toolchain as linked above.

If you’re building for iOS, you should have:

* [ldid](http://iphonedevwiki.net/index.php/Ldid)
* xz-utils (or lzma)
* An iOS SDK (this will be discussed in the next section)

On macOS, you can install like so (after installing [Homebrew](https://brew.sh/)):

```console
$ brew install ldid
```

On iOS, ldid is installed as part of the Theos Dependencies package. On Linux and Cygwin, ldid is included as part of the toolchain download.

Again — don’t forget the prerequisites listed in the table above!

## Installation
> **⚠️ Warning if installing Theos on iOS:** Unless you’re using the Electra jailbreak, the `git` command below currently won’t work due to a GitHub change that made it incompatible with Telesphoreo’s version of OpenSSL. saurik is working on an update. See [issue #293](https://github.com/theos/theos/issues/293) for more information and a workaround.

Decide where you want to install Theos. We recommend `~/theos` or a similar directory that is in a location you can write to without having to use `sudo`.

You must set the `$THEOS` variable in your environment, and export it so `make` will see its value when you run it. Create or edit `~/.bash_profile` (or equivalent for your shell of choice) and add this, replacing `~/theos` with the full path to where Theos is installed if you chose another place:

```bash
export THEOS=~/theos
```

For this change to take effect, you must restart your shell. The easiest way to do this is to simply open a new tab and close the previous one, or kill the app in the taskswitcher (iOS). Do `echo $THEOS` on your shell to check if this is working.

On iOS, `~/.profile` can be an alternative file used in place of `~/.bash_profile`, so rename the file if the check above didn't work.

Then proceed to clone Theos to this directory:

```console
$ git clone --recursive https://github.com/theos/theos.git $THEOS
```

Don’t forget the `--recursive` flag. The Theos repository contains submodules, and this flag will clone them for you. If you forget this, you can fix it by running the updater:

```console
$ $THEOS/bin/update-theos
```

Other common places are `/opt/theos` and `/var/theos`. If you want to use /var, /opt, or any other similar directory, keep in mind that they will not be writable except by root. You must use `sudo` on the above command, and then change the owner to yourself:

```console
$ sudo chown -R $(id -u):$(id -g) $THEOS
```

While it is possible to download Theos using the “Download ZIP” button on GitHub, this is discouraged as it will make it hard to update Theos in future.

In order to use `make troubleshoot`, you need to install Ghostbin’s [ghost.sh](https://ghostbin.com/ghost.sh) script.

```console
$ curl https://ghostbin.com/ghost.sh -o $THEOS/bin/ghost
$ chmod +x $THEOS/bin/ghost
```

If building for iOS, you will need an SDK. Xcode always provides the latest iOS SDK, but as of Xcode 7.3, it no longer includes private frameworks you can link against. This may be an issue when developing tweaks. You can get a patched SDK from [our SDKs repo](https://github.com/theos/sdks) — click the “Download ZIP” button in the top-right, extract it, and copy the SDK(s) you want into the `sdks/` folder inside Theos.

If you are using Theos on Linux (or Cygwin or Windows Subsystem for Linux) you have to use an SDK for iOS 9.3 or below. the iOS 9.2 SDK is the version you should use on those platforms.

### Toolchain
macOS and iOS should have been covered with the prerequisites. For Linux setups (WSL included) you'll have to do one more step:

```console
$ mkdir -p $THEOS/toolchain
$ unzip ios-toolchain_clang+llvm+ld64_latest_linux_x86_64.zip -d $THEOS/toolchain
```

## Updating
Theos utilises a [rolling release](https://en.wikipedia.org/wiki/Rolling_release) model, meaning the latest commit to the Git repo is the latest version of Theos available. Occasionally, you should update Theos. This can be done with:

```console
$ $THEOS/bin/update-theos
```

If you get a “no such file or directory” error, you’re probably not using a recent version of Theos that added this script. As an alternative, switch to a directory containing a Theos makefile and then run:

```console
$ make update-theos
```

(Both commands do the same thing.)

If you experience problems, updating Theos is the first thing you should do. This makes it a lot easier to track down the problem if you ask someone for help.

If you see the following when running the command:

```
make: *** No rule to make target 'update-theos'.  Stop.
```

…then you’re using a legacy version of Theos. Read onto the next section.

## Switching from DHowett’s or rpetrich’s Theos
Refer to [[Upgrading from legacy Theos]].
