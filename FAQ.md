## FAQ
### Fork of Theos? Why?
There has been a lack of development on Theos recently, there were already a few patches I made for myself and left uncommitted for a few years, and some fixes were critically needed for new SDKs and for dependencies such as dpkg-deb to continue to work with Telesphoreo’s horribly outdated dpkg. From there it grew to other features I desired, and wanted to see more of the community taking advantage of, such as debug builds by default, optimisation of particular file types in release builds, and Swift compilation. The fork has significantly matured to the point that it would be hard to merge it back into the original Theos repo. It also pulls in commits from [rpetrich’s Theos fork](https://github.com/rpetrich/theos) which was created when Theos was still in its early days, moving away from its original name of “iphone-framework”. This fork also merges [[many other forks|Features]] by the community.

And besides, why would it be bad to give back so many improvements to the community?

Moving along…

### How do I install Theos, or switch to this Theos fork?
See [[Installation]].

### All of my package versions contain `+debug` now! How do I stop this?
This fork encourages using debug builds by default, rather than release builds. Building as a debug build provides more ease in using debuggers on the code, enables debug logging with `HBLogDebug()`, and makes syslog output colored so it’s easier to find in a sea of other log messages.

To disable debug mode, pass `DEBUG=0` as part of your command line to `make`. When you make a release build with `FOR_RELEASE=1` or `FINALPACKAGE=1`, debug will also be disabled (and the build number is also removed so your version number is cleaner).

### `NSLog` is deprecated? What? How do I log now?
Like the above situation, this fork also aims to encourage developers to specify a “level” along with their logs. The levels are:

* **`HBLogDebug`** – used to log data that is useful during development, but not useful in a released package.
* **`HBLogInfo`** – used to log informational messages that do not indicate a problem.
* **`HBLogWarn`** – used to indicate a problem that can be recovered from
* **`HBLogError`** – used to indicate a problem that can not be recovered from.

All of these macros are used exactly the same way as NSLog – all that changes is the *name* of the function you call.

Here’s a practical example of all of these in use:

```logos
- (UIImage *)iconForBundleIdentifier:(NSString *)bundleIdentifier {
    HBLogInfo(@"looking up icon for %@", bundleIdentifier);

    SBApplication *app = [[%c(SBApplicationController) sharedInstance] applicationForBundleIdentifier:bundleIdentifier];

    if (!app) {
        HBLogError(@"could not retrieve app instance for %@", bundleIdentifier);
        return nil;
    }

    HBLogDebug(@"app for %@ is %@", bundleIdentifier, app);

    SBApplicationIcon *appIcon = [[[%c(SBApplicationIcon) alloc] initWithApplication:app] autorelease];
    UIImage *icon = [appIcon getIconImage:SBApplicationIconFormatSpotlight];

    if (icon) {
        return icon;
    } else {
        HBLogWarn(@"no spotlight icon was found. falling back to default icon");

        icon = [appIcon getIconImage:SBApplicationIconFormatDefault];

        if (!icon) {
            HBLogError(@"couldn't get a default icon - giving up");
        }

        return icon;
    }
}
```

It is hoped that this will encourage more carefully considered logging that is convenient to both the developer and users reading their syslogs. Not logging at all only makes it harder for you to track down issues – please consider using this!

### I built a package using Swift, but it crashes on my phone. Why?
This fork is ready in terms of supporting *building* Swift packages; however, the Swift runtime is not available in Cydia. The reasoning for this is that many of us have been unable to find a definitive license detailing distribution of these libraries and thus it is best to stay away from releasing them.

There *is* good news, however. Swift 2.0 is to be released as an open source product, and that means soon runtime libraries will begin to become available in Cydia.

Until then, you can play around with Swift by copying the libraries to your device manually:

```console
$ ssh device "mkdir -p /usr/lib/libswift/1.2"
$ rsync -rav "$(xcode-select -print-path)/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphoneos" device:/usr/lib/libswift/1.2
```

This assumes you’re using Xcode 6.3 or 6.4, which both include Swift 1.2. Change the version in the path if not. You can find out what version your copy of Xcode has with `swift --version`.

### Windows support? Whoa. How do I use that?
Refer to [the sharedInstance post](http://sharedinstance.net/2013/12/build-on-windows/) on setting this up.

### Where’s the `theos` symlink?
This fork has opted to not use this symlink any longer because of a few reasons. First, there are a fair few “noisy” files dropped in the root of a project by standard Theos; this fork prefers to have as few of those as possile (and you may have noticed some are now stashed into the `.theos` directory). Second, this feels like a hack, and it can be very different between developers and even between each of a developer’s devices as Theos can be located anywhere. It’s also easy to unintentionally commit this symlink to source control or not know that this shouldn’t be committed to source control.

Of course, this does mean you must set and export `$THEOS` in your environment. Do this in your shell’s profile or environment script (for instance `~/.bash_profile` or `~/.zshrc`) to ensure it’s always set and you don’t have to worry about it. It might look like this:

```console
$ export THEOS=/usr/local/theos
```

If this is undesireable, you can tell NIC to revert to symlinking `theos`:

```console
$ echo 'link_theos = "1"' >> ~/.nicrc
```

See [[nicrc(5)]] for more details on `.nicrc`.