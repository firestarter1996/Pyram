# Pyram: Nagram with a few quality of life fixes

Pyram is a fork that builds [Nagram](https://github.com/NextAlone/Nagram) with these changes on top of the newest upstream release. The forum topic typing fix that started this fork is now part of upstream Nagram itself (release 1253), so it is no longer carried here.

## What is changed

* **Drafts never lose text.** A cloud draft update used to clear the input field when the open topic had no stored draft, and a late server echo could replace a newer, longer local draft with an older, shorter one. Both are fixed: a server draft older than the local one is ignored, and what you type is also kept in a small local copy, written on every change (debounced), so it survives a crash, a process kill or a reboot. A cloud draft that comes back shorter or empty never replaces the text you typed. A different draft written later on another device still wins.
* **Select all** is back in the input field's long press menu.
* **Topic tabs** put the topic you sent to most recently on the left. Pinned topics stay first in their own order; topics you never sent to keep the server order.
* **Page up and page down** buttons sit with the other round buttons on the right of a chat and scroll by one screen. Each shows only when the chat can scroll that way.
* **Fast scroll with dates.** A thumb on the right edge appears while a chat scrolls and can be dragged; a bubble beside it shows the date of the messages at the top of the screen. It covers the messages already loaded; dragging to the top loads older ones.
* **Smooth bot progress bars.** A bot keyboard that works as a media player (a time button like `🔊 0:07 / 4:36` and a row of 16 emoji cells, both with callback data `tts:pos`) is only edited every few seconds. The time and a solid bar now move smoothly between edits.

Each change is one patch in [patches/](patches/), in the order they are applied.

## How updates work

[`.github/workflows/typing-build.yml`](.github/workflows/typing-build.yml) runs every day:

1. It looks up the newest NextAlone/Nagram release.
2. If that release was already built with the current patches, it stops.
3. It checks every patch. If a patch no longer applies, or upstream already contains the change, the run fails and opens an issue here instead of shipping a stale build.
4. It builds the arm64 release APK with Nagram's own build steps, signs it with this fork's own key, checks the signer and publishes a GitHub Release tagged `<upstream release>-typing-<patch set>`.

Nagram's native library checks the APK signer at startup against a SHA-1 compiled into `TMessagesProj/jni/integrity/integrity.cpp`. The workflow swaps in this fork's own certificate SHA-1 before the native build, and stops with an issue if that check ever changes shape upstream.

## Installing

Download the APK from [Releases](https://github.com/firestarter1996/Nagram/releases), or add `https://github.com/firestarter1996/Nagram` to [Obtainium](https://github.com/ImranR98/Obtainium) for automatic updates.

The package name is the same as official Nagram (`xyz.nextalone.nagram`), but the signing key is different, so it cannot update an official Nagram install in place. Official Nagram has to be removed first (that deletes its local data; your chats stay in your Telegram account).

Signing certificate SHA-256:
`8D:47:25:57:BB:6D:7A:18:D9:DF:B8:6C:89:28:EA:D2:2A:13:D1:9E:C5:10:B1:71:8F:14:37:B9:B6:C8:31:3A`

## Source

Everything except this branch is the upstream Nagram code. The changes live on the `features` branch, one commit per patch. Nagram is licensed under GPL 3.0, see the upstream [LICENSE](https://github.com/NextAlone/Nagram/blob/main/LICENSE).

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=firestarter1996/Nagram&type=Date)](https://star-history.com/#firestarter1996/Nagram&Date)
