# Kanata keymap: MacBook Air JIS

This is a draft Kanata `.kbd` file based on
[`forest-tech/my-karabiner-elements`](https://github.com/forest-tech/my-karabiner-elements),
with Vim-friendly ASCII symbol placement. It has **not** been tested on the
actual MacBook keyboard.

## Configuration

- `mac-jis.kbd`: built-in Apple JIS keyboard only.
- Three thumb actions: `eisu` tap/Command hold, `Space` tap/Control hold,
  `kana` tap/Shift hold.
- `Caps` tap = Escape; hold = Symbol1/Numbers; hold Caps + kana = Symbol2.
- No Home Row Mods or Navigation layer.
- Emacs Control remaps from the previous Karabiner config are **not** enabled
  because they would override shortcuts in the terminal, tmux and Neovim.

## Enable/disable each feature

Edit the `FEATURE SWITCHES` block at the top of `mac-jis.kbd`:

| Variable | ON (default) | OFF |
| --- | --- | --- |
| `feature-eisu` | `@eisu-cmd` | `eisu` |
| `feature-space` | `@space-ctrl` | `spc` |
| `feature-kana` | `@kana-shift` | `kana` |
| `feature-symbol1` | `@caps-symbol1` | `esc` |
| `feature-symbol2` | `@hold-symbol2` | `XX` |

For faster Space/Ctrl, change `feature-space` to `@space-ctrl-fast`.
It may cause unintended Ctrl shortcuts during quick ordinary typing.
`feature-symbol1 = esc` also makes Symbol2 unreachable. These flags are
applied when you restart Kanata or reload the configuration.

The timing variables are in the same `defvar` section. Start with the defaults,
then tune the hold times if you notice accidental shortcuts or excessive delay.

## Symbol layer outputs

The key shown above each symbol is the physical alphabet key. A dot (`-` in
this table) means the original key has no dedicated symbol mapping.

| Physical | Q | W | E | R | T | Y | U | I | O | P |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Caps = Symbol1 | - | @ | % | ` | ! | { | 7 | 8 | 9 | , |
| Caps + kana = Symbol2 | - | @ | % | ~ | & | } | 7 | 8 | 9 | . |

| Physical | A | S | D | F | G | H | J | K | L | ; |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Symbol1 | - | ' | / | : | * | ( | 4 | 5 | 6 | < |
| Symbol2 | - | " | ? | ; | # | ) | 4 | 5 | 6 | > |

| Physical | Z | X | C | V | B | N | M | , | . | / | Space |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Symbol1 | - | $ | = | - | \| | [ | 1 | 2 | 3 | / | 0 |
| Symbol2 | - | ^ | + | _ | \ | ] | 1 | 2 | 3 | ? | 0 |

The 32 printable ASCII punctuation symbols and digits `0-9` are covered
across these two layers. JIS layout is used at the macOS input-source level:
this file sends keycodes, not Unicode glyphs.

## Setup / test on macOS

1. Ensure the matching Karabiner-DriverKit-VirtualHIDDevice driver is installed
   for the version of Kanata you use. Follow the
   [official macOS setup](https://github.com/jtroo/kanata/blob/main/docs/setup-macos.md).
2. **Disable Karabiner-Elements' active remapping / keyboard grabbing** before
   starting Kanata to avoid interference. Do not uninstall the shared driver.
3. Check the exact built-in keyboard name:
   `kanata --list`. The file currently uses
   `"Apple Internal Keyboard / Trackpad"`. Edit `macos-dev-names-include`
   if your device appears under another name.
4. Check configuration syntax (does not require device grabs):
   `kanata --check --cfg ./mac-jis.kbd`.
5. Start Kanata following official macOS setup instructions, e.g.
   `sudo kanata --cfg ./mac-jis.kbd`.
6. Test in a plain text editor (ABC input source), then in Ghostty and Neovim.

### Smoke test

- Tap `Caps` -> Escape; hold `Caps + F` -> `:`.
- `Caps + D` -> `/`; `Caps + kana + D` -> `?`.
- `Caps + X` -> `$`; `Caps + kana + X` -> `^`.
- `Caps + G` -> `*`; `Caps + kana + G` -> `#`.
- `Caps + H` -> `(`; `Caps + kana + H` -> `)`.
- `Caps + U/I/O` -> `7/8/9`; `Caps + Space` -> `0`.
- `Caps + B` -> `|`; `Caps + kana + B` -> backslash.
- Hold `Space` for approximately 190 ms, then press `F` -> Control+F.
- Test repeated `j` in Neovim (should behave like normal repeated `j`).
- Check both ABC and Japanese input modes for the Yen/backslash key and IME behavior.

**Known limitations:** No macOS live keyboard test has been performed.
Apple JIS symbol output depends on the active input source. Yen/backslash,
pipe, underscore and braces are especially worth verifying. The Symbol2
trigger requires holding Caps and kana at once, which may be awkward to reach;
this is intentionally left as an initial ergonomic experiment.

## Sources

- [Kanata configuration guide](https://jtroo.github.io/config.html)
- [Kanata macOS setup](https://github.com/jtroo/kanata/blob/main/docs/setup-macos.md)
- [Previous Karabiner map](https://github.com/forest-tech/my-karabiner-elements)
