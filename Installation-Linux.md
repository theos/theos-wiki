This guide will help you install Theos on your Linux machine, Linux within Windows via [Windows Subsystem for Linux](https://docs.microsoft.com/windows/wsl), [Google Cloud Shell](https://console.cloud.google.com/cloudshell).

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **Linux** <br> **Windows 10** | Linux kernel 3.16 <br> Windows 10 build 14393 | Linux, iOS |

1. Install the following prerequisites:

* fakeroot, git, perl, build-essential (or equivalent for your distro)

```bash
sudo apt-get install fakeroot git perl build-essential
```

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

```console
curl https://developer.angelxwind.net/Linux/ios-toolchain_clang%2bllvm%2bld64_latest_linux_x86_64.zip -o toolchain.zip
unzip toolchain.zip -d $THEOS/toolchain
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
