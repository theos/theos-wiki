This guide will help you install Theos on your Linux machine, Linux within Windows via [Windows Subsystem for Linux](https://docs.microsoft.com/windows/wsl), [Google Cloud Shell](https://console.cloud.google.com/cloudshell).

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **Linux** <br> **Windows 10** | Linux kernel 3.16 <br> Windows 10 build 14393 | Linux, iOS |

1. Follow the instructions at <http://apt.llvm.org> to add the correct clang-6.0 source for your Linux distro

2. Install the following prerequisites:

        sudo apt-get install fakeroot git perl clang-6.0 build-essential

    <sup>
    <sup>*</sup> build-essential or equivalent for your distro.
    </sup>

	On Google Cloud Shell:

		sudo apt install bash coreutils git grep make openssh-client perl rsync sed

3. Set up the `THEOS` environment variable:

        echo "export THEOS=~/theos" >> ~/.profile

    For this change to take effect, you must restart your shell. Open a new tab and do `echo $THEOS` on your shell to check if this is working.

4. Clone Theos to your device:

        git clone --recursive https://github.com/theos/theos.git $THEOS

5. Get the toolchain:

        curl https://kabiroberai.com/toolchain/download.php?toolchain=ios-linux -Lo toolchain.tar.gz
        tar xzf toolchain.tar.gz -C $THEOS/toolchain

6. Get an iOS SDK:

    You can get patched SDKs from [our SDKs repo](https://github.com/theos/sdks).

		curl -LO https://github.com/theos/sdks/archive/master.zip
		unzip master.zip -d $THEOS/sdks

7. Set up ghostbin script

		curl https://ghostbin.com/ghost.sh -o $THEOS/bin/ghost
		chmod +x $THEOS/bin/ghost

8. Install the Swift toolchain (optional)

		curl https://kabiroberai.com/toolchain/download.php?toolchain=swift-ubuntu1610 -Lo swift-toolchain.tar.gz
		tar xzf swift-toolchain.tar.gz -C $THEOS/toolchain

    Note that the minimum SDK version required to compile Swift code is currently iOS 11.2.