## Officially supported platforms

Theos aims to be able to work on, and build for, the following platforms.

- **[[iOS|Installation-iOS]]** on iOS 5 and up.
- **[[macOS|Installation-macOS]]** on Mavericks (10.9) and up.
- **[[Linux/WSL|Installation-Linux]]** on Linux kernel 3.16 or Windows 10 build 14393, and up.
- **[[Cygwin|Installation-Cygwin]]** on Windows XP and up.

Other platforms (or versions older than listed above) may work, but be aware that they are unsupported. Theos may work on them now, but it may not in future. If you think we should officially support a platform not listed here, [get in touch](https://github.com/theos/theos/issues/new).

## Updating

Theos utilises a [rolling release](https://en.wikipedia.org/wiki/Rolling_release) model, meaning the latest commit to the Git repo is the latest version of Theos available. Occasionally, you should update Theos. This can be done with:

	$THEOS/bin/update-theos

If you get a “no such file or directory” error, you’re probably not using a recent version of Theos that added this script. As an alternative, switch to a directory containing a Theos makefile and then run:

	make update-theos

(Both commands do the same thing.)

If you experience problems, updating Theos is the first thing you should do. This makes it a lot easier to track down the problem if you ask someone for help.

If you see the following when running the command:

```
make: *** No rule to make target 'update-theos'.  Stop.
```

…then you’re using a legacy version of Theos. Read onto the next section.

## Switching from DHowett’s or rpetrich’s Theos

Refer to [[Upgrading from legacy Theos]].
