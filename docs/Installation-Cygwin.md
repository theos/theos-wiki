This guide will help you install Theos on your Windows (7, 8, and 8.1) machine via Cygwin.

Please consider using [Windows Subsystem for Linux](Installation-Linux) instead if possible. Cygwin works but is quite limited.

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **Windows** | XP | iOS |

All the commands shown on the following instructions are meant to be run as the "user" user, _not_ **root**. Similarly, Theos is also meant to be run as a normal user, _not_ **root**.

1. Install the following prerequisites:

	* [Cygwin](https://cygwin.com/install.html)
	* git (under Devel)
	* make (under Devel)
	* ca-certificates (under Net)
	* openssh (under Net)
	* perl (under Perl)
	* python (under Python)

1. Set up the `THEOS` environment variable:

		echo "export THEOS=~/theos" >> ~/.profile

	For this change to take effect, you must restart your shell. Open a new tab and do `echo $THEOS` on your shell to check if this is working.

1. Clone Theos to your device:

		git clone --recursive https://github.com/theos/theos.git $THEOS

1. Get the toolchain:

	On 32 bit:

		git clone git://github.com/coolstar/iOSToolchain4Win.git $THEOS/toolchain/windows/iphone

	On 64 bit:

		git clone -b x86_64 git://github.com/coolstar/iOSToolchain4Win.git $THEOS/toolchain/windows/iphone

1. Get an iOS SDK:

	You can get patched SDKs from [our SDKs repo](https://github.com/theos/sdks).

		curl -LO https://github.com/theos/sdks/archive/master.zip
		TMP=$(mktemp -d)
		unzip master.zip -d $TMP
		mv $TMP/sdks-master/*.sdk $THEOS/sdks
		rm -r master.zip $TMP
		
	You can extract original SDKs from old versions of Xcode available at [Apple Developer Downloads](https://developer.apple.com/download/all/).
	For example you can get the iOS 5.1 SDK the following path in Xcode 4.3.1:
	
	```
	/Applications/Xcode 4.3.1.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS5.1.sdk
	```

