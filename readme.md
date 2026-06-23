# betterxSound

A maintained, drop-in replacement for [Xogy/xsound](https://github.com/Xogy/xsound) (MIT).

betterxSound keeps the original API — same exports, events, and resource name `xsound` — so you replace your existing `xsound` folder and change nothing else.

## Preview

[![betterxSound preview](https://img.youtube.com/vi/ErJoF7W6jCI/maxresdefault.jpg)](https://youtu.be/ErJoF7W6jCI)

## What is fixed

- **YouTube memory leak.** Sound iframes are torn down correctly, the duration probe no longer spawns a second hidden YouTube player, and an idle DUI recycle reclaims the native/GPU memory CEF would otherwise hold. RAM stays bounded instead of climbing until the browser crashes.
- **Pooled YouTube players (v1.1).** YouTube players are pooled and reused across songs via `loadVideoById`, capped by `Config.youtubePoolMax`. Back-to-back playback (a jukebox) no longer keeps spawning new player instances CEF cannot free — memory stays flat no matter how many tracks play.
- **Anti-abuse.** The interact-sound emulator and crewphone trigger events are rate-limited, volume-clamped, and file/URL validated server-side, with an optional ACE gate for server-wide broadcasts. Players can no longer spam sounds at everyone.
- **Self-contained.** howler (2.2.4), jQuery and DOMPurify are bundled locally — no CDN dependency.

## Install

1. Replace your existing `xsound` resource with this one. Keep the folder name `xsound`.
2. `ensure xsound` in your server config.
3. Optional: configure `config.idleDuiRecycleSec`, `config.youtubePoolMax`, and `config.security` in `config.lua`.

No code changes are needed in resources that already use xSound.

## Configuration highlights

- `config.idleDuiRecycleSec` — seconds of silence before the audio browser is recycled to reclaim memory. `0` disables.
- `config.youtubePoolMax` — max simultaneous YouTube players; reused across songs via `loadVideoById`.
- `config.security` — rate limit, volume cap, file/URL validation, and an optional `broadcastAce` permission for server-wide sounds.

## Docs

Full documentation: https://docs.whereiaml.com/docs/betterxsound

## Upstream

These fixes are also submitted upstream so everyone benefits, not just users of this fork: [Xogy/xsound#74](https://github.com/Xogy/xsound/pull/74). If it merges, prefer upstream xSound.

## Credits

Original xSound by Xogy — https://github.com/Xogy/xsound. Licensed under MIT; this fork keeps the original license and adds modifications under the same terms.
