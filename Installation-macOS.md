This guide will help you install Theos on your macOS device.

| Platform | Minimum OS version | Targets supported
|----------|--------------------|-------------------|
| **macOS** | Mavericks (10.9) | macOS, iOS, watchOS, tvOS |

1. Install the following prerequisites:

* [Homebrew](https://brew.sh/)
* [Xcode](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12)<sup>1</sup> is mandatory. The Command Line Tools package isn’t sufficient for Theos to work. Xcode includes toolchains for all Apple platforms.
* [ldid](http://iphonedevwiki.net/index.php/Ldid)
* xz-utils (or lzma)

<sup>
<sup>1</sup> Xcode 5.0 or newer. Xcode 4.4 supported, but only when building for ARMv6 (1st/2nd generation iPhone/iPod touch).
</sup>

```console
brew install ldid xz
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

4. Get an iOS SDK:

Xcode always provides the latest iOS SDK, but as of Xcode 7.3, it no longer includes private frameworks you can link against. This may be an issue when developing tweaks. You can get a patched SDK from [our SDKs repo](https://github.com/theos/sdks) — click the “Download ZIP” button in the top-right, extract it, and copy the SDK(s) you want into the `sdks/` folder inside Theos.
