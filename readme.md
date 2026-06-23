# betterxSound

A maintained, drop-in replacement for [Xogy/xsound](https://github.com/Xogy/xsound) (MIT).

betterxSound keeps the original API — same exports, events, and resource name `xsound` — so you replace your existing `xsound` folder and change nothing else.

## Preview

[![betterxSound preview](https://img.youtube.com/vi/ErJoF7W6jCI/maxresdefault.jpg)](https://youtu.be/ErJoF7W6jCI)

## What is fixed

- **YouTube memory — mitigated, not magic.** The leak lives in YouTube's embedded player and how CEF holds it; it cannot be made fully leak-free while using the YouTube iframe (the upstream author hit the same wall). This fork reduces it where it can: the duration probe no longer spawns a second hidden player, players are **pooled and reused across songs** via `loadVideoById` (capped by `Config.youtubePoolMax`), and an **idle recycle** reclaims memory when nothing is playing. For continuous / jukebox playback that keeps memory flat. It does **not** solve the case of a long-lived sound (a looped siren, a global stream, anything not destroyed) — that keeps a player alive, so memory still grows there. If you need genuinely leak-free YouTube audio, the only real path is extracting a direct audio stream server-side (e.g. yt-dlp) and playing that instead of an embed.
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
