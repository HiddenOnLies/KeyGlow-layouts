# KeyGlow workshop: keyboard layouts

Exact keyboard layouts for [KeyGlow](https://github.com/HiddenOnLies/KeyGlow), contributed by the
community. KeyGlow already detects most keyboards automatically through OpenRGB; this repository
is for the ones whose drawing doesn't come out right (unusual right columns, extra keys, split
boards…).

## How KeyGlow uses it

- The app downloads **[`layouts.json`](layouts.json)**, one file with every layout, at most once a
  day, and looks up your keyboard **locally**. It never sends your keyboard's ID anywhere.
- It's cached, so KeyGlow works offline, and every layout is **validated** before use.
  Layouts are plain JSON data, never code.
- It can be turned off in KeyGlow: *Settings → Download keyboard layouts from the KeyGlow workshop*.

## Adding your keyboard

1. In KeyGlow, open *Settings* and click **Export this layout**. You get a `.json` with your
   keyboard's keys, its USB ID and its name.
2. Fix any key that's in the wrong place. Positions are in key units: `x` and `y` are the
   top-left corner, `w`/`h` the size (default 1). For example a 2.25-unit Shift:
   `{"id": "LSHIFT", "x": 0, "y": 3, "w": 2.25}`.
3. Put it in [`layouts/`](layouts) with a descriptive name (`brand-model.json`).
4. Check it and rebuild the bundle (needs KeyGlow from source: `pip install git+https://github.com/HiddenOnLies/KeyGlow`):

   ```bash
   keyglow workshop layouts --bundle layouts.json
   ```

5. Open a pull request. The automatic check runs the same validation.

Set `"verified": true` **only if you tested the layout on the keyboard itself**, and say how in
`"verified_on"`. Unverified layouts are welcome too; KeyGlow tells users they haven't been
tested, and a verified layout for the same keyboard always wins.

## Format

```json
{
  "title": "HyperX Alloy Origins 65",
  "usb": ["03f0:038f"],
  "names": ["HyperX Alloy Origins 65"],
  "verified": true,
  "verified_on": "OpenRGB 1.1, Windows 11",
  "keys": [{"id": "ESC", "x": 0, "y": 0}, {"id": "LSHIFT", "x": 0, "y": 3, "w": 2.25}]
}
```

- `usb`: `"vendor:product"` IDs in hex, how KeyGlow recognizes the keyboard (preferred).
- `names`: OpenRGB device names, used when no USB ID matches.
- `name` (optional): an internal ID. Reuse a KeyGlow built-in layout name (like
  `alloy_origins_65`) when your layout has the same shape, so existing profiles match it exactly.
- `caps_lock_indicator` (optional): `true` if the keyboard has its own Caps Lock light that still
  works while OpenRGB controls the colors, `false` if it doesn't. When it's `false` or missing,
  KeyGlow lights the Caps Lock key while Caps Lock is on.
- `keys`: KeyGlow key IDs (`ESC`, `A`, `LSHIFT`, `NUM_7`, `MEDIA_PLAY`…). Run
  `keyglow layout ansi_full --json` to see a full example.

## License

[MIT](LICENSE). By contributing a layout you agree to share it under the same license.
