---
layout: post
title: "Dock disappeared in MacOS Catalina"
previous: https://gonfva.medium.com/dock-disappeared-in-macos-catalina-65cd3c053fd2
subtitle: The Windows Vista of Apple
date: 2019-10-16T18:03:00
categories: [Developer, got ya]
tags:
  - Developer
---

Yes. I feel your pain. That little bar at the bottom of your screen is no longer visible after you upgraded to macOS Catalina.

Well, here I am, to help you before Apple does.

Open a terminal.

> How? — you may ask — my dock doesn’t exist. I don’t know how to open anything

Cmd+Space to open spotlight, type terminal, but don’t press enter, yet, because the default for terminal sometimes is… terminal-notifier. How the hell spotlight decides those things? I don’t know.

Once you have a terminal, type the following

```
defaults delete com.**apple**.**dock**; killall **Dock**
```

You will now see your dock. However after restarting you will face the same issue.

So before restarting, I opened Finder, and then I launched the Go dialog into the following folder

```
~/Library/Application Support
```

![](/img/1*wG8F3yAXZIV_A6zNekEzvQ.png)

To open the Go dialog (“Go to the folder”) press CMD+SHIFT+G

Once in the folder ~/Library/Application Support, you have to delete the folder Dock.

And now you can restart.

Note: The solution comes from [here](https://discussions.apple.com/thread/250718639), but it is not the accepted answer.
