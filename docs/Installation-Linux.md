This guide will help you install Theos on your Linux machine, Linux within Windows via [Windows Subsystem for Linux](https://docs.microsoft.com/windows/wsl), or [Google Cloud Shell](https://console.cloud.google.com/cloudshell).

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **Linux** <br> **Windows 10** | Linux kernel 3.16 <br> Windows 10 build 14393 | Linux, iOS |

All the commands shown on the following instructions are meant to be run as the "user" user, _not_ **root**. Similarly, Theos is also meant to be run as a normal user, _not_ **root**.

1. Install the following prerequisites:

	On Linux or WSL:

		sudo apt-get install fakeroot git perl unzip build-essential libtinfo5

	<sup>
	<sup>*</sup> build-essential and libtinfo5 or the equivalents for your distro.
	</sup>

	Additionally on WSL:

		sudo update-alternatives --set fakeroot /usr/bin/fakeroot-tcp

	On Google Cloud Shell:

		# Setup llvm repository
		echo -e "deb http://apt.llvm.org/stretch/ llvm-toolchain-stretch-6.0 main\ndeb-src http://apt.llvm.org/stretch/ llvm-toolchain-stretch-6.0 main" | sudo tee -a /etc/apt/sources.list.d/llvm.list
		wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | sudo apt-key add -
		sudo apt update

		# Install dependencies
		sudo apt install bash coreutils fakeroot git grep make openssh-client perl rsync sed

1. Set up the `THEOS` environment variable:

		echo "export THEOS=~/theos" >> ~/.profile

	For this change to take effect, you must restart your shell. Open a new tab and do `echo $THEOS` on your shell to check if this is working.

1. Clone Theos to your device:

		git clone --recursive https://github.com/theos/theos.git $THEOS

1. Get a toolchain:

		curl -LO https://github.com/sbingner/llvm-project/releases/latest/download/linux-ios-arm64e-clang-toolchain.tar.lzma
		TMP=$(mktemp -d)
		tar --lzma -xvf linux-ios-arm64e-clang-toolchain.tar.lzma -C $TMP
		mkdir -p $THEOS/toolchain/linux/iphone
		mv $TMP/ios-arm64e-clang-toolchain/* $THEOS/toolchain/linux/iphone/
		rm -r linux-ios-arm64e-clang-toolchain.tar.lzma $TMP

1. Get an iOS SDK:

	You can get patched SDKs from [our SDKs repo](https://github.com/theos/sdks).

		curl -LO https://github.com/theos/sdks/archive/master.zip
		TMP=$(mktemp -d)
		unzip master.zip -d $TMP
		mv $TMP/sdks-master/*.sdk $THEOS/sdks
		rm -r master.zip $TMP

1. Install the Swift toolchain (optional):

		curl https://kabiroberai.com/toolchain/download.php?toolchain=swift-ubuntu-latest -Lo swift-toolchain.tar.gz
		tar xzf swift-toolchain.tar.gz -C $THEOS/toolchain
		rm swift-toolchain.tar.gz

	Note that the minimum SDK version required to compile Swift code is currently iOS 11.2.
