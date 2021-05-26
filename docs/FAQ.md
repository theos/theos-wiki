## All of my package versions contain `+debug` now! How do I stop this?
Theos encourages using debug builds by default, rather than release builds. Building as a debug build provides more ease in using debuggers on the code, enables debug logging with `HBLogDebug()`, and makes syslog output colored so it’s easier to find in a sea of other log messages.

To disable debug mode, pass `DEBUG=0` as part of your command line to `make`. When you make a release build with `FINALPACKAGE=1`, debug will also be disabled (and the build number is also removed so your version number is cleaner).

## Where’s the `theos` symlink?
Theos has opted to stop using this symlink by default because of a few reasons. First, there are a fair few “noisy” files dropped in the root of a project by standard Theos; this fork prefers to have as few of those as possible (and you may have noticed some are now stashed into the `.theos` directory). Second, this can be very different between developers and even between each of a developer’s devices as Theos can be located anywhere. It’s also easy to unintentionally commit this symlink to source control or not know that this shouldn’t be committed to source control.

Of course, this does mean you must set and export `$THEOS` in your environment. Do this in your shell’s profile or environment script (for instance `~/.bash_profile`, or `~/.zshrc`) to ensure it’s always set and you don’t have to worry about it. It might look like this:

```bash
export THEOS=/usr/local/theos
```

If this is undesirable, you can tell NIC to revert to symlinking `theos`:

```console
$ echo 'link_theos = "1"' >> ~/.nicrc
```

See [[nicrc(5)]] for more details on `.nicrc`.

## How do I use Swift in my projects?
This information has moved to the [[Swift]] page.