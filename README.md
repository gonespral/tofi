# tofi (fork)

[tofi](https://github.com/philj56/tofi) is a fast, simple
[dmenu](https://tools.suckless.org/dmenu/) /
[rofi](https://github.com/davatorium/rofi) replacement for wlroots-based
Wayland compositors such as [Sway](https://github.com/swaywm/sway): it reads
lines on stdin, lets you filter them as you type and prints the one you pick.
See upstream for the full documentation, themes and benchmarks.

This fork is based on upstream **v0.9.1** and adds the changes below, one
commit each, mostly for using tofi as a fullscreen menu from scripts. They
apply to the pango text backend (used when `--font` is a font name rather
than a file path).

## Changes

Look:
- **Blinking cursor** (530ms, like GTK); typing restarts the blink.
- **Ellipsis**: results too long for the window end in "…" instead of being
  cut off.
- **Input scrolls** to keep the cursor in view when the typed text is longer
  than the window.
- **Placeholder**: with nothing typed the cursor is drawn beside the
  placeholder, not over its first character.
- **Scrollbar** under the prompt when the results don't all fit.
- **Marquee**: a highlighted result that's too long scrolls back and forth.
- **Box mode**: `--box-width` / `--box-height` (px or %) with `--dim-color`
  draw the menu as a centred box in a dimmed (e.g. fullscreen) window.

Behaviour:
- **Row-by-row scrolling**: Up / Down move one row at a time instead of
  jumping a page, and wrap round at either end.
- **`--linger=MS`** (and `--linger-cancel`): after a pick the result is
  printed at once, but the window stays as plain background until another
  surface takes the keyboard, so chained menus never flash.
- **`--selection=N`**: start with result N highlighted.
- **`--live-prefix=P --live-cmd=CMD`**: while the input starts with P, the
  results are the output lines of CMD (the rest of the input in
  `$TOFI_LIVE`), e.g. a calculator.
- **`--hover-file=FIFO`**: the highlighted result is written to the FIFO
  whenever it changes, for live previews.
- **`--fade-in=MS` / `--fade-out=MS`**: fade the window in and out
  (`wp_alpha_modifier_v1`, ease-out cubic); the fade out is skipped when
  another surface takes the keyboard.
- **Mouse**: hovering a result highlights it, a click picks it, a click
  outside the box closes the menu, and the wheel moves the highlight like
  Up / Down. A normal arrow cursor (`cursor-shape-v1`).

## Building

```sh
meson setup build
ninja -C build
sudo ninja -C build install
```

Dependencies are the same as upstream (freetype, harfbuzz, cairo, pango,
wayland, libxkbcommon; meson, scdoc and wayland-protocols to build).

## License

MIT, as upstream: see [LICENSE](LICENSE).
