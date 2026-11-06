# 👆👆🔊 Double Tap Read Aloud

A plugin for Nagram XF. Double tap a bot message that has a Read Aloud button and it starts reading. Double tap it again to pause, and again to resume.

## Why I made it

My bridge bot puts a Read Aloud button under every message it posts, and the button turns into a little player while the message is read. Hitting a button while walking is fiddly. A double tap anywhere on the message is easier.

## What it changes

* Double tap a message with a Read Aloud button: the plugin presses that button for you, so reading starts.
* Double tap a message that is being read (its keyboard shows the player): the plugin presses the player, which pauses or resumes.
* Double tap a message whose player shows Done or Stopped: it is read again from the start.
* Messages without a Read Aloud button keep XF's normal double tap, except in the chats you list in the settings, where a double tap on them does nothing (so no stray reactions land in the bot's group).

Settings: a switch for the whole thing, a switch for the quiet chats, and the list of quiet chats (dialog ids).

Buttons are recognized by their callback data: `tts:read` starts reading, `tts:pos` is the play and pause toggle, `tts:pause` and `tts:resume` are the older reply panel. Any bot that uses the same data works.

## Install

1. Use the plugin build of Nagram XF and turn on the plugin engine (Settings, N Settings, Plugins).
2. Download `double_tap_read_aloud.plugin` from the latest release.
3. Send it to your Saved Messages, tap it and choose install. You can also use the plus button on the Plugins screen.
4. Make sure Double Tap Read Aloud is switched on, then reopen your chats.

## How it works

The chat screen's item click listener in `ChatActivity` answers `hasDoubleTap` and runs `onDoubleTap`. The plugin hooks `RecyclerListView.setOnItemClickListener`, and the first time a chat sets that listener it hooks those two methods once. On a tap it reads the message's inline keyboard (a few buttons), and on a double tap it calls `SendMessagesHelper.sendCallback` with the chosen button, exactly what a finger on the button does. Nothing runs per frame, only per tap. At startup one background lookup (about 50 ms, off the main thread) finds the listener of a chat that was already open. Errors go to logcat under the tag `DoubleTapRead`.

## Tested

Nagram XF 12.10.1 plugin build, plugin SDK 1.4.6.7, on a Pixel 6 Pro running the same build and settings as my daily phone, in my real bridge group.

* A test build pressed the player button of a stopped panel on a double tap. The bridge bot received and answered that callback within a second, and nothing played.
* A double tap on a message with a Read Aloud button picked `tts:read`. The choices for a playing, finished and old style panel were checked against copies of the bridge's keyboards.
* In the bridge group, a double tap on a message without the button did nothing: no menu, no reaction, no translation. Single taps still open the message menu.
* Scrolling a long topic with the plugin on and off: no difference in frame times or CPU.

## License

MIT

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=firestarter1996/xf-double-tap-read-aloud&type=Date)](https://star-history.com/#firestarter1996/xf-double-tap-read-aloud&Date)
