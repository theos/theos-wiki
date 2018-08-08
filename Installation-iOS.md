This guide will help you install Theos on your iOS jailbroken device.

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **iOS** | 5.0 | iOS |

1. Install the following prerequisites:

* [CoolStar’s repository](https://coolstar.org/publicrepo/)
* [Sam Bingner’s repository](http://repo.bingner.com/) if on iOS 11
* [Theos Dependencies](http://moreinfo.thebigboss.org/moreinfo/depiction.php?file=theosdependenciesDp) (package on BigBoss repo)

2. Set up the `THEOS` environment variable:

```bash
echo "export THEOS=~/theos" >> ~/.profile
```

For this change to take effect, you must restart your shell. Kill the terminal app in the taskswitcher then re-open the terminal app and do `echo $THEOS` on your shell to check if this is working.

3. Clone Theos to your device:

```console
git clone --recursive https://github.com/theos/theos.git $THEOS
```

> **⚠️ Warning if installing Theos on iOS 11:** Unless you’re using the Electra jailbreak, the `git` command currently won’t work due to a GitHub change that made it incompatible with Telesphoreo’s version of OpenSSL. saurik is working on an update. See [issue #293](https://github.com/theos/theos/issues/293) for more information on the following workaround.

```console
git clone --recursive https://github.com/theos/theos.git
rsync -aP theos/ mobile@your.iphone.ip:/var/mobile/theos
```

4. Get an iOS SDK:

You can get patched SDKs from [our SDKs repo](https://github.com/theos/sdks).

```console
curl https://github.com/theos/sdks/archive/master.zip -o master.zip
unzip master.zip -d $THEOS/sdks
```

5. Set up ghostbin script

```console
curl https://ghostbin.com/ghost.sh -o $THEOS/bin/ghost
chmod +x $THEOS/bin/ghost
```
