## I have a project that uses legacy Theos. How do I update it for the latest Theos?
There should be no breaking changes for almost all projects. Take a look at the [[Upgrading from legacy Theos]] page for a list of the most important changes and how to upgrade.

## All of my package versions contain `+debug` now! How do I stop this?
Theos encourages using debug builds by default, rather than release builds. Building as a debug build provides more ease in using debuggers on the code, enables debug logging with `HBLogDebug()`, and makes syslog output colored so it’s easier to find in a sea of other log messages.

To disable debug mode, pass `DEBUG=0` as part of your command line to `make`. When you make a release build with `FINALPACKAGE=1`, debug will also be disabled (and the build number is also removed so your version number is cleaner).

## Where’s the `theos` symlink?
Theos has opted to stop using this symlink by default because of a few reasons. First, there are a fair few “noisy” files dropped in the root of a project by standard Theos; this fork prefers to have as few of those as possile (and you may have noticed some are now stashed into the `.theos` directory). Second, this feels like a hack, and it can be very different between developers and even between each of a developer’s devices as Theos can be located anywhere. It’s also easy to unintentionally commit this symlink to source control or not know that this shouldn’t be committed to source control.

Of course, this does mean you must set and export `$THEOS` in your environment. Do this in your shell’s profile or environment script (for instance `~/.bash_profile` or `~/.zshrc`) to ensure it’s always set and you don’t have to worry about it. It might look like this:

```console
$ export THEOS=/usr/local/theos
```

If this is undesirable, you can tell NIC to revert to symlinking `theos`:

```console
$ echo 'link_theos = "1"' >> ~/.nicrc
```

See [[nicrc(5)]] for more details on `.nicrc`.

## I built a package using Swift, but it crashes on my phone. Why?
Theos is ready in terms of supporting *building* Swift packages, however the Swift runtime is not available in Cydia.

Until this happens, you can play around with Swift by copying the libraries to your device manually:

```console
$ ssh device "mkdir -p /usr/lib/libswift/3.0.2"
$ rsync -aP "$(xcode-select -print-path)/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphoneos" device:/usr/lib/libswift/3.0.2
```

This assumes you’re using Xcode 8.0, which includes Swift 3.0.2. Change the version in the path if not. You can find out what version your copy of Xcode has with `swift --version`.

## Can Swift be used in tweaks, frameworks, and other bundles?
It’s possible, and you can try it out right now (note: no Logos support), but there is a fundamental problem with Swift as it stands – its [ABI](https://en.wikipedia.org/wiki/Application_binary_interface) is not stable. Each release of Swift to date has made major changes that make it impossible to use, for instance, a binary built with Swift 2.3 with the Swift 3.0 runtime libraries. It is also not possible to have multiple runtime versions loaded into a process at a time. Using a mismatched runtime version can cause dyld loading errors, meaning your code won’t run at all, or crashes due to changes to the runtime’s behavior.

Imagine this now with tweaks – your tweak targets an app built with Swift 3.0, and so the Swift 3.0 runtime is already loaded into it. You write your own tweak, also using Swift 3.0. This works, but later the app is built with a hypothetical newer version of Xcode that uses Swift 3.1, and your tweak stops working or crashes. Alternatively, the app is written in Objective-C, and a tweak using Swift 3.0 is loaded into it. However, your tweak is using Swift 2.3, and loads after the prior tweak, and again your code simply doesn’t run or causes a crash.

Due to this, it won’t be possible for tweaks to be built in Swift for the foreseeable future. Even after Swift 4.0 stabilises the ABI, there will still be apps around that are written in older versions of Swift, making it only viable in specific situations where it’s guaranteed that only Swift 4.0 and newer will be used in the app.

Of course, this only applies to tweaks. Non-library projects such as tools and apps are the main use case for Swift, and you can feel free to use Swift here.