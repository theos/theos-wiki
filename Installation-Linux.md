This guide will help you install Theos on your Linux machine, Linux within Windows via [Windows Subsystem for Linux](https://docs.microsoft.com/windows/wsl), [Google Cloud Shell](https://console.cloud.google.com/cloudshell).

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **Linux** <br> **Windows 10** | Linux kernel 3.16 <br> Windows 10 build 14393 | Linux, iOS |

1. Install the following prerequisites:

        sudo apt-get install fakeroot git perl build-essential

    <sup>
    <sup>*</sup> build-essential or equivalent for your distro.
    </sup>

	On Google Cloud Shell:

		sudo apt install bash coreutils git grep make openssh-client perl rsync sed

2. Set up the `THEOS` environment variable:

        echo "export THEOS=~/theos" >> ~/.profile

    For this change to take effect, you must restart your shell. Open a new tab and do `echo $THEOS` on your shell to check if this is working.

3. Clone Theos to your device:

        git clone --recursive https://github.com/theos/theos.git $THEOS

4. Get the toolchain:

        curl https://developer.angelxwind.net/Linux/ios-toolchain_clang%2bllvm%2bld64_latest_linux_x86_64.zip -o toolchain.zip
        unzip toolchain.zip -d $THEOS/toolchain

5. Get an iOS SDK:

    You can get patched SDKs from [our SDKs repo](https://github.com/theos/sdks).

		curl -LO https://github.com/theos/sdks/archive/master.zip
		unzip master.zip -d $THEOS/sdks

    It is recommended that you use an SDK for iOS 9.3 or below due to the toolchain not being up to date as on other platforms.

6. Set up ghostbin script

		curl https://ghostbin.com/ghost.sh -o $THEOS/bin/ghost
		chmod +x $THEOS/bin/ghost

<details>
<summary>Optional: Installing Swift</summary>
<br>
In the future, the Swift toolchain for Linux shall be available in a pre-packaged format. However, it is currently possible to build an iOS-compatible Swift toolchain for Linux using the <a href="https://github.com/kabiroberai/swift-toolchain-linux">kabiroberai/swift-toolchain-linux</a> repository. 
<br><br>
After following the instructions in the repository's README.md, run the following command
<br><br>

    tar xzf </path/to/toolchain.tar.gz> -C $THEOS/toolchain

Note that the minimum SDK version required to compile Swift code is currently iOS 11.2. As a result, you may need a newer version of the iOS toolchain, since the one listed above does not support this SDK.
</details>