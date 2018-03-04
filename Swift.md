Theos is able to compile [Swift](https://swift.org/) files, including using them alongside files in Objective-C and other languages. Currently, Theos only supports Swift on macOS, using Xcode’s copy of the Swift toolchain, but in future it will expand to support Apple’s official Linux releases, as well as the unofficial community releases for Windows and possibly Cygwin.

The Swift runtime is currently on BigBoss under libswif4. Binaries that use Swift will fail to load if the runtime libraries aren’t installed.

You can also manually copy the libraries from Xcode to your device:

```console
$ ssh device "mkdir -p /usr/lib/libswift/4.0.3"
$ rsync -aP "$(xcode-select -print-path)/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphoneos/" device:/usr/lib/libswift/4.0.3
```

This assumes you’re using Xcode 9.2, which includes Swift 4.0.3. Change the version in the path if not. You can find out what version your copy of Xcode has with `swift --version`.

## Variables
* `<instance>_SWIFTFLAGS` or `<file>_SWIFTFLAGS` *string*. Default: empty. Custom flags to pass to the Swift compiler for all files or a specific file (respectively).
* `<instance>_BRIDGING_HEADER` *filename*. Default: `<instance>-Bridging-Header.h`. The [Objective-C bridging header](https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/MixandMatch.html) to be imported into all Swift files during compilation.

## Caveat: Libraries
It’s possible to use Swift for library instances (i.e. tweaks, dylibs, frameworks, and other bundles), and you can try it out right now (note: no Logos support yet), but there is a fundamental problem with Swift as it stands – its [ABI](https://en.wikipedia.org/wiki/Application_binary_interface) is not stable. Each release of Swift to date has made major changes that make it impossible to use, for instance, a binary built with Swift 2.3 with the Swift 3.0 runtime libraries. It is also not possible to have multiple runtime versions loaded into a process at a time. Using a mismatched runtime version can cause dyld loading errors, meaning your code won’t run at all, or crashes due to changes to the runtime’s behavior.

Imagine this now with tweaks – your tweak targets an app built with Swift 3.0, and so the Swift 3.0 runtime is already loaded into it. You write your own tweak, also using Swift 3.0. This works, but later the app is built with a newer version of Xcode that uses Swift 3.1, and your tweak stops working or crashes. Alternatively, the app is written in Objective-C, and a tweak using Swift 3.0 is loaded into it. However, your tweak is using Swift 2.3, and loads after the prior tweak, so again your code simply doesn’t run or causes a crash.

Due to this, it won’t be possible for tweaks to be built in Swift until Swift 5. The upcoming Swift 5.0 stabilises the ABI, and likely will be able to coexist with older Swift runtimes loaded into the same process, so it will be possible to safely write tweaks in Swift 5.

Of course, as mentioned above this only applies to libraries. Non-library projects such as tools and apps are the main use case for Swift, and you can feel free to use Swift here.