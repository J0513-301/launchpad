[README.md](https://github.com/user-attachments/files/33123172/README.md)
# Online Launchpad: First of the Year (Equinox)

A keyboard sampler for Skrillex's *First of the Year (Equinox)*. Your keyboard becomes a 48-pad launchpad, and each key plays a section of the original song so you can rebuild it live. It is a standalone rebuild of the Online Launchpad site: plain HTML, CSS, and JavaScript, with no server, accounts, or saving.

## Quick start

1. Unzip the folder.
2. Open `index.html` in a modern browser (Chrome, Edge, Firefox, or Safari).
3. Wait for "Ready", then press keys.

It works straight from your computer (`file://`), so there is nothing to install or host. If you hear nothing on the first press, press a key once more. Browsers only start audio after a keypress or tap.

## Controls

The pad grid is the 4 by 12 block of keys from `1` to `Shift`:

| Row | Keys |
| --- | --- |
| 1 | `1` `2` `3` `4` `5` `6` `7` `8` `9` `0` `-` `=` |
| 2 | `Q` `W` `E` `R` `T` `Y` `U` `I` `O` `P` `[` `]` |
| 3 | `A` `S` `D` `F` `G` `H` `J` `K` `L` `;` `'` `Enter` |
| 4 | `Z` `X` `C` `V` `B` `N` `M` `,` `.` `/` `Shift` |

- **Sound packs:** four packs, each with its own set of samples. `←` is pack 1, `↑` is pack 2, `↓` is pack 3, and `→` is pack 4. The on-screen buttons do the same thing.
- **Stop all:** `Esc` or the button.
- **Touch and mouse:** tap or click any pad. Multi-touch works. The last pad in the grid (labeled "tap") has no keyboard key, so it is touch and mouse only.
- **Hold-to-play pads:** pads marked with a dot only sound while held. All other pads play their sample once, and pressing the same pad again restarts it.
- **Linked pads:** some pads form groups where starting one cuts off the others, so related parts don't pile up.
- **Pad states:** amber fill means held, an amber outline means the sample is still sounding, and a faded pad has no sample in this pack.

Keys use physical positions, so the layout is the same on non-QWERTY keyboards.

## Files

| File | What it does |
| --- | --- |
| `index.html` | Page markup. Loads the other three files. |
| `style.css` | Styling, with light and dark themes that follow your system setting. |
| `app.js` | All the logic: key mapping, Web Audio playback, hold-to-play, linked pads, pack switching. |
| `samples.js` | The audio (base64 mp3s) and the pad-to-sample map for each pack. About 2 MB. |

`samples.js` must load before `app.js`. The only external request is one Google Font, with a system fallback if you are offline.

## How it works

On load, `app.js` decodes every sample into an audio buffer with the Web Audio API, which keeps the delay between keypress and sound very short. Pressing a pad starts a new buffer source for it. Releasing a hold-to-play pad fades it out over a few milliseconds to avoid clicks. Sounds started in one pack keep playing if you switch to another pack.

## Using your own samples

All song data lives in `samples.js`:

```js
const SAMPLES = { "ab12cd34ef": "<base64 mp3>", ... };

const RAW_PACKS = [
  {
    map:    [ "ab12cd34ef", null, ... ],  // 48 entries, one per pad, or null for empty
    hold:   [4, 5, 6],                    // pads that only sound while held
    linked: [[0, 12, 13]]                 // groups where only one pad plays at a time
  },
  // ...four packs in total
];
```

Pads are numbered row by row: 0 is the `1` key, 11 is `=`, 12 is `Q`, and so on through 47, the touch-only pad. To encode a file, run `base64 -w0 sound.mp3` on Linux or `base64 -i sound.mp3` on macOS. Packs with no samples have their button hidden automatically.

## Not included

This rebuild covers the core instrument only. The original site's user accounts, saving and loading, and recording are left out, because they depended on a server.

## Credits

- The original **Online Launchpad** (Ruby on Rails, GitHub: Cbo11/OBSLP) is by Daniel Weber and credited as MIT licensed on its site. This rebuild follows its pad mapping, hold-to-play keys, and linked groups.
- The samples come from Nev's project file for *First of the Year (Equinox)* by Skrillex, as credited by the original project. They belong to their owners, so this is meant for personal use. Please don't publish or redistribute the audio.
