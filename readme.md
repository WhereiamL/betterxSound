# betterxSound

A maintained, drop-in replacement for [Xogy/xsound](https://github.com/Xogy/xsound) (MIT).

betterxSound keeps the original API — same exports, events, and resource name `xsound` — so you replace your existing `xsound` folder and change nothing else.

## What is fixed

- **YouTube memory leak.** Sound iframes are torn down correctly, the duration probe no longer spawns a second hidden YouTube player, and an idle DUI recycle reclaims the native/GPU memory CEF would otherwise hold. RAM stays bounded instead of climbing until the browser crashes.
- **Anti-abuse.** The interact-sound emulator and crewphone trigger events are rate-limited, volume-clamped, and file/URL validated server-side, with an optional ACE gate for server-wide broadcasts. Players can no longer spam sounds at everyone.
- **Self-contained.** howler (2.2.4), jQuery and DOMPurify are bundled locally — no CDN dependency.

## Install

1. Replace your existing `xsound` resource with this one. Keep the folder name `xsound`.
2. `ensure xsound` in your server config.
3. Optional: configure `config.youtubeApiKey`, `config.idleDuiRecycleSec`, and `config.security` in `config.lua`.

No code changes are needed in resources that already use xSound.

## Configuration highlights

- `config.youtubeApiKey` — optional YouTube Data API v3 key. Set it to show track length without spawning a hidden video player. Empty by default.
- `config.idleDuiRecycleSec` — seconds of silence before the audio browser is recycled to reclaim memory. `0` disables.
- `config.security` — rate limit, volume cap, file/URL validation, and an optional `broadcastAce` permission for server-wide sounds.

## Docs

Full documentation: https://docs.whereiaml.com/docs/betterxsound

## Credits

Original xSound by Xogy — https://github.com/Xogy/xsound. Licensed under MIT; this fork keeps the original license and adds modifications under the same terms.
