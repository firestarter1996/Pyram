# Nagram with the forum topic typing fix

This branch builds [Nagram](https://github.com/NextAlone/Nagram) with one small fix: in a group with forum topics, "is typing" now shows in every topic where someone is typing, instead of only one of them.

## The bug

`MessagesController.updatePrintingStrings()` loops over every topic that has someone typing. For each topic it made a brand new per dialog map and stored it in `newStrings`, so each topic replaced the one before it. Only the last topic in the hash walk kept its typing text. Stock Telegram for Android has the same code.

The fix builds the per dialog maps once, before the topic loop. See [patches/0001-forum-topic-typing.patch](patches/0001-forum-topic-typing.patch). The same change is offered upstream as a pull request.

## How updates work

[`.github/workflows/typing-build.yml`](.github/workflows/typing-build.yml) runs every day:

1. It looks up the newest NextAlone/Nagram release.
2. If that release was already built with the current patches, it stops.
3. It checks every patch. If a patch no longer applies, or upstream already contains the change, the run fails and opens an issue here instead of shipping a stale build.
4. It builds the arm64 release APK with Nagram's own build steps, signs it with this fork's own key, checks the signer and publishes a GitHub Release tagged `<upstream release>-typing-<patch set>`.

## Installing

Download the APK from [Releases](https://github.com/firestarter1996/Nagram/releases), or add `https://github.com/firestarter1996/Nagram` to [Obtainium](https://github.com/ImranR98/Obtainium) for automatic updates.

The package name is the same as official Nagram (`xyz.nextalone.nagram`), but the signing key is different, so it cannot update an official Nagram install in place. Official Nagram has to be removed first (that deletes its local data; your chats stay in your Telegram account).

Signing certificate SHA-256:
`8D:47:25:57:BB:6D:7A:18:D9:DF:B8:6C:89:28:EA:D2:2A:13:D1:9E:C5:10:B1:71:8F:14:37:B9:B6:C8:31:3A`

## Source

Everything except this branch is the upstream Nagram code. The fix itself lives on the `fix-forum-topic-typing` branch. Nagram is licensed under GPL 3.0, see the upstream [LICENSE](https://github.com/NextAlone/Nagram/blob/main/LICENSE).

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=firestarter1996/Nagram&type=Date)](https://star-history.com/#firestarter1996/Nagram&Date)
