This guide will help you install Theos on your iOS jailbroken device.

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **iOS** | 5.0 | iOS |

1. Install the following prerequisites:

    * [CoolStar’s repository](https://coolstar.org/publicrepo/)
    * [Sam Bingner’s repository](http://repo.bingner.com/)
    * Theos Dependencies (package on BigBoss repo, relies on the previous repositories being installed first)

2. Set up the `THEOS` environment variable:

        echo "export THEOS=~/theos" >> ~/.profile

    For this change to take effect, you must restart your shell. Kill the terminal app in the taskswitcher then re-open the terminal app and do `echo $THEOS` on your shell to check if this is working.

3. Clone Theos to your device:

        git clone --recursive https://github.com/theos/theos.git $THEOS

4. Get an iOS SDK:

    You can get patched SDKs from [our SDKs repo](https://github.com/theos/sdks).

        curl https://github.com/theos/sdks/archive/master.zip -o master.zip
        unzip master.zip -d $THEOS/sdks

5. Set up ghostbin script

        curl https://ghostbin.com/ghost.sh -o $THEOS/bin/ghost
        chmod +x $THEOS/bin/ghost
