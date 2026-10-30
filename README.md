# Clip Shoutout

A clip shoutout for OBS, as a browser source. When a mod, a VIP or the broadcaster types `!so somestreamer` in
chat, a comic-book shoutout card pops up with that streamer's picture, name, link and what they were last
playing, and one of their clips plays in it.

- The clip is picked **at random**, and never the one that streamer got last time, as long as they have more
  than one.
- Shoutouts given while one is on screen **wait in a queue** and play one after another, so `!so a`, `!so b`,
  `!so a` shows all three (with two different clips of `a`).
- If they have **no clips**, or the channel can't be found, the card still shows on its own for a few seconds.

Lives at <https://clips.ixitxachitl.com/>.

## Setup

In OBS: **Add → Browser**, URL `https://clips.ixitxachitl.com/?channel=yourchannel`, at your canvas size (e.g.
1920×1080). The background is transparent inside OBS. Tick **Control audio via OBS** if you want the clips'
sound on its own mixer slider.

Right-click the source → **Interact** and move the mouse over it:

- The box in the bottom-right corner shouts out a name you type (or yourself, if it's empty), and **Skip**
  skips the shoutout on screen.
- The **gear** opens the settings: your Twitch / Kick channel, TikTok or YouTube, who can give shoutouts, how
  clips are picked, and the card's position and size.

Every setting can also be a URL parameter, which overrides it. The full list is in the comment at the top of
`index.html`. `?demo=1` gives a shoutout every few seconds, for trying it out.

## Platforms

| Chat     | Read                          | Shoutouts show                                                    |
| -------- | ----------------------------- | ----------------------------------------------------------------- |
| Twitch   | anonymously                   | Twitch channels, with a Twitch clip                              |
| Kick     | anonymously                   | Kick channels, with a Kick clip                                   |
| YouTube  | Data API key, or a ws bridge  | the name looked up on Twitch (or Kick, or not at all: `lookup`)   |
| TikTok   | a bridge (e.g. TikFinity)     | the same as YouTube                                               |

`!so kick.com/name`, `!so twitch.tv/name`, `!so kick:name` and `!so twitch:name` pick the platform whatever
chat it's typed in.

Twitch channels and clips are looked up with the same public, logged-out requests twitch.tv makes for its own
visitors, so there's nothing to log in to or register. If a clip's video won't play, Twitch's own clip player
takes over. Kick's come from its public channel API and play through [hls.js](https://github.com/video-dev/hls.js).

## Chat commands

| Command          | Who                                        | What                                             |
| ---------------- | ------------------------------------------ | ------------------------------------------------ |
| `!so name`       | mods, VIPs and the broadcaster (`who`)     | a shoutout for `name` (`@name` works too)        |
| `!soskip`        | mods and the broadcaster                   | skip the shoutout on screen                      |
| `!soskip all`    | mods and the broadcaster                   | skip it and empty the queue                      |

Both can be renamed in the settings. **Who can use it** can be just the broadcaster, mods, mods and VIPs (the
default) or everyone, and **These people can too** adds particular names.

Custom integrations (Streamer.bot, etc.) can call `window.clips.shoutout(name, { platform })`, `.skip()` and
`.clear()`.
