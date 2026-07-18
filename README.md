# ROT-1 Extended — MIDI Corruptor

An expanded fork of the original ROT-1 MIDI Corruptor with additional controls, improved visualization, and quality-of-life features.

**Format-aware byte rot for Standard MIDI Files. Headers stay valid, the music doesn't.**

Drop in a `.mid` file, push the faders, and listen to it fall apart — then download the wreckage and open it in any DAW. Everything runs client-side in your browser; nothing is uploaded anywhere.

**▶ Live demo:** https://moxie-coder.github.io/ROT-1/

![ROT-1 Extended screenshot](https://i.imgur.com/kZL91Q4.png)

## Why "format-aware"?

Randomly flipping bytes in a MIDI file almost never produces glitchy music — it produces silence or a parser error. The format is fragile in three specific ways:

- **Delta times are variable-length quantities.** Flip the high bit of a VLQ byte and the parser desyncs from that point on.
- **Running status.** Files omit repeated status bytes, so corrupting one status byte misinterprets everything after it.
- **Track length headers.** `MTrk` chunks declare their byte length up front; insertions and deletions break parsing immediately.

ROT-1 Extended instead fully parses the file (VLQs, running status, meta and sysex events included), mutates only the musical payload, and re-serializes with correct headers, lengths, and delta times. The output is a 100% valid SMF that any sequencer will open — it just sounds wrong in exactly the way you dialed in.

## Why this fork?

ROT-1 Extended began as a fork of the original browser MIDI corruptor.

The original project demonstrated format-aware MIDI corruption extremely well, but intentionally kept the interface simple. This fork expands on that idea with additional controls, visualization improvements, reproducible seeded corruption, quality-of-life features, and an interface designed for experimentation.

The goal isn't realism—it's making controlled musical destruction fun.

## The corruption bank

| Fader | What it rots |
|---|---|
| **Pitch chaos** | Randomly transposes notes (up to ±3 octaves at full tilt) |
| **Velocity chaos** | Randomizes note dynamics |
| **Time smear** | Jitters note timing by up to a full beat |
| **Note decay** | Notes simply vanish |
| **Instrument roulette** | Rerolls GM program changes — injects them on tracks that never had any |
| **Tempo warp** | Bends existing tempo events and injects random BPM lurches at high settings |

All corruption is **seeded**: the same seed and parameter values always produce the exact same corrupted MIDI.

You can:

- Re-roll the seed while keeping all parameters.
- Randomize any individual fader with its 🎲 button.
- Reset every parameter back to zero at any time.

This makes it easy to experiment while still being able to reproduce interesting results.

Note-offs follow their note-ons through pitch changes and deletions, so corrupted files never have stuck notes.

## Features

- Format-aware MIDI corruption engine
- Live piano-roll visualization
- Built-in Web Audio preview
- Original vs corrupted A/B playback
- Download valid Standard MIDI Files
- Per-fader randomization (🎲)
- Seeded, deterministic corruption
- One-click seed reroll
- Detailed mutation log
- Demo MIDI included
- Completely offline
- Single HTML file
- No dependencies

## Planned Features

- Corruption presets
- Favorite seeds
- Seed history
- Randomize all parameters
- Shareable corruption settings
- Additional corruption algorithms
- More visualization improvements

## Usage

No installation is required.

Simply visit the GitHub Pages build, or download the single `index.html` file and open it locally in any modern browser.

Because the entire application is self-contained, it also works completely offline.

```bash
git clone https://github.com/YOURUSER/midi-corruptor.git
cd midi-corruptor
xdg-open index.html   # or just double-click it
```

1. Drop a `.mid` file onto the deck (or click **Load demo loop**)
2. Push faders until it sounds appropriately broken
3. **▶ Corrupted** to preview, **⬇ Download** to export

## Limitations

- The preview synth is oscillator-based, not a soundfont — instrument roulette sounds far more dramatic in a real DAW than in the browser preview
- SMPTE-division files are handled with approximated timing
- Sysex and most meta events pass through untouched (only tempo gets warped)

## License

MIT — corrupt responsibly.
