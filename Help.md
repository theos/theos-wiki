## Places you can ask for help
* Check [Theos/Troubleshooting](http://iphonedevwiki.net/index.php/Theos/Troubleshooting) on iPhone Dev Wiki first – common issues are listed here.
* Connect to [IRC](http://iphonedevwiki.net/index.php/How_to_use_IRC) (#theos on irc.saurik.com) and ask your question there.
* Post a question at [/r/jailbreakdevelopers](https://www.reddit.com/r/jailbreakdevelopers).
* Post a question on [Stack Overflow](https://stackoverflow.com/questions/tagged/theos) using the “theos” tag.

If you think you’ve found a bug in Theos, [file an issue](https://github.com/theos/theos/issues). Please make sure that you are actually reporting a bug, and not an error in your system or project configuration, or your code. If you’re not sure, ask at one of the options listed above.

## Advice
First and foremost, make sure you know how to program. Generally, the help places listed above will refer you to programming tutorials if it seems you’re asking basic programming questions. Try searching the web for iOS or Android app development tutorials, or buy a book. They will start with an idea and guide you through writing the app. Don’t feel like you need to release apps you make while initially learning to program, unless you’re confident in them and want to pay for an Apple or Google developer account. If you don’t have access to a Mac or hackintosh, Android development is an equally good alternative to iOS development — Android Studio is available for Windows, Linux, and macOS — and will help you learn many similar concepts.

When asking a question, provide as much information as you possibly can. It is better that you provide too much information, than to provide fairly little and not end up getting any answers. Providing only your code is not enough to understand what the issue is.

Make sure your question isn’t an [XY problem](http://xyproblem.info/). This means that instead of asking about your *problem*, you ask about a specific *solution* to the problem. This makes it harder to answer your question, and others may ask you to explain more about your problem. Refer to the link for a more detailed explanation.

Be prepared to take on criticism. Experienced programmers may say something that has the ability to upset you if taken the wrong way. Keep in mind that they are providing criticism to help you become a better programmer. There’s no such thing as perfect, but you can get pretty close by listening to the advice of people that were once in your position.

When sharing a snippet of code, it is usually best to post it to a paste site. We recommend [Ghostbin](https://ghostbin.com/), but any similar site is fine. On IRC, be aware that pasting more than a few lines at once will cause the server to kick (disconnect) you as it is considered flooding. On Stack Overflow and GitHub, you can paste your code directly into the body, and then select it and click “Code” on the toolbar to ensure it is correctly formatted.

## make troubleshoot
You can run `make troubleshoot` from within a project directory to upload some diagnostic information to [Ghostbin](https://ghostbin.com/), and copy the link to your clipboard. If `ghost` is not installed, you can do so by executing the following:

```bashsession
$ curl https://ghostbin.com/ghost.sh -o /usr/local/bin/ghost
$ chmod +x /usr/local/bin/ghost
```

The troubleshoot command will execute `make clean all messages=yes`.