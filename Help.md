## Places you can ask for help
* Check [Theos/Troubleshooting](http://iphonedevwiki.net/index.php/Theos/Troubleshooting) on iPhone Dev Wiki first – common issues are listed here.
* Connect to [IRC](http://iphonedevwiki.net/index.php/How_to_use_IRC) (#theos on irc.saurik.com) and ask your question there.
* Post a question at [/r/jailbreakdevelopers](https://www.reddit.com/r/jailbreakdevelopers).
* Post a question on [Stack Overflow](https://stackoverflow.com/questions/tagged/theos) using the “theos” tag.

If you think you’ve found a bug in Theos, [file an issue](https://github.com/theos/theos/issues). Please make sure that you are actually reporting a bug, and not an error in your system or project configuration, or your code. If you’re not sure, ask at one of the options listed above.

## Advice
Provide as much information as you possibly can. It is better that you provide too much information, than to provide fairly little and not end up getting any answers.

Make sure your question isn’t an [XY problem](http://xyproblem.info/). This means that instead of asking about your problem, you ask about a specific solution to the problem. Refer to the link for a more detailed explanation. This makes it harder to answer your question, and others may ask you to explain more about your problem.

Be prepared to take on criticism. Experienced programmers may say something that has the ability to upset you if taken the wrong way. However, keep in mind that they are providing criticism to help you become a better programmer. There’s no such thing as perfect, but you can get pretty close by listening to the advice of people that were once in your position.

## make troubleshoot
You can run `make troubleshoot` from within a project directory to upload some diagnostic information to [Ghostbin](https://ghostbin.com/), and copy the link to your clipboard. If `ghost` is not installed, you can do so by executing the following:

```bashsession
$ curl https://ghostbin.com/ghost.sh -o /usr/local/bin/ghost
$ chmod +x /usr/local/bin/ghost
```

The troubleshoot command will execute `make clean all messages=yes`.