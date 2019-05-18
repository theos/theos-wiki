This guide will help you install Theos on your iOS jailbroken device.

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **iOS** | 5.0 | iOS |

All the commands shown on the following instructions are meant to be run as the "user" user, _not_ **root**. Similarly, Theos is also meant to be run as a normal user, _not_ **root**.

1. Install the following prerequisites:

	* [Sam Bingner’s repository](http://repo.bingner.com/)
	* Theos Dependencies (package on BigBoss repo, relies on the previous repository being installed first)

1. Set up the `THEOS` environment variable:

		echo "export THEOS=~/theos" >> ~/.profile

	For this change to take effect, you must restart your shell. Kill the terminal app in the taskswitcher then re-open the terminal app and do `echo $THEOS` on your shell to check if this is working.

1. Clone Theos to your device:

		git clone --recursive https://github.com/theos/theos.git $THEOS

1. Get the toolchain:

	Theos Dependencies installs iOS Toolchain.

1. Get an iOS SDK:

	You can get patched SDKs from [our SDKs repo](https://github.com/theos/sdks).

		curl -LO https://github.com/theos/sdks/archive/master.zip
		unzip master.zip -d $THEOS/sdks
		rm master.zip

1. Set up ghostbin script (optional):

		curl https://ghostbin.com/ghost.sh -o $THEOS/bin/ghost
		chmod +x $THEOS/bin/ghost

1. Install the Swift toolchain (optional):

	`swift-toolchain` is on the BigBoss repo.

	Note that the minimum SDK version required to compile Swift code is currently iOS 11.2.
