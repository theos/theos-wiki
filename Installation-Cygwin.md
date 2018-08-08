This guide will help you install Theos on your Windows (XP, Vista, 7, 8, and 8.1) machine via Cygwin.

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **Windows** | XP | iOS |

1. Install the following prerequisites:

* [Cygwin](https://cygwin.com/install.html)
* git (under Devel)
* make (under Devel)
* ca-certificates (under Net)
* openssh (under Net)
* perl (under Perl)
* python (under Python)

2. Set up the `THEOS` environment variable:

```bash
echo "export THEOS=~/theos" >> ~/.profile
```

For this change to take effect, you must restart your shell. Open a new tab and do `echo $THEOS` on your shell to check if this is working.

3. Clone Theos to your device:

```console
git clone --recursive https://github.com/theos/theos.git $THEOS
```

4. Get the toolchain:

If on 32 bit:

```console
git clone git://github.com/coolstar/iOSToolchain4Win.git $(THEOS)/toolchain/windows/iphone
```

If on 64 bit:

```console
git clone -b x86_64 git://github.com/coolstar/iOSToolchain4Win.git $(THEOS)/toolchain/windows/iphone
```

5. Get an iOS SDK:

You can get patched SDKs from [our SDKs repo](https://github.com/theos/sdks).

```console
curl https://github.com/theos/sdks/archive/master.zip -o master.zip
unzip master.zip -d $THEOS/sdks
```

6. Set up ghostbin script

```console
curl https://ghostbin.com/ghost.sh -o $THEOS/bin/ghost
chmod +x $THEOS/bin/ghost
```
