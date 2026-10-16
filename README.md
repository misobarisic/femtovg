[![Crates.io](https://img.shields.io/crates/v/femtovg?color=4d76ae)](https://crates.io/crates/femtovg)
[![API](https://docs.rs/femtovg/badge.svg)](https://docs.rs/femtovg)
[![dependency status](https://deps.rs/repo/github/femtovg/femtovg/status.svg)](https://deps.rs/repo/github/femtovg/femtovg)
[![rust: 1.70](https://img.shields.io/badge/MSRV-1.70-success.svg)](#msrv)
[![Discord](https://img.shields.io/discord/691716054560407592)](https://discord.gg/V69VdVu)

<h2 align="center">femtovg</h2>
<p align="center">GPU Antialiased 2D vector drawing library written in Rust.</p>

- [Cargo crate](https://crates.io/crates/femtovg)
- [Book](https://femtovg.github.io)
- [Join the femtovg Discord channel](https://discord.gg/V69VdVu)

---

Ported from https://github.com/memononen/nanovg

Most of the implementation is the same as the original C code with some bug fixes, some features added and several parts have been made more Rust-y. Rendering is done via one OpenGl (ES) 3.0+ backend.

## COLRv1 color fonts (fork)

The optional `colrv1` feature adds CPU rasterization of OpenType COLRv1 paint
graphs through Skrifa 0.44 and tiny-skia 0.12 into the existing color glyph
atlas. It works with positioned glyph runs and the existing text layout API;
other glyphs continue through the existing Swash/outline paths. The default
CPAL palette and foreground color, variation coordinates, transforms, clips,
gradients and isolated composites are supported. Output uses straight RGBA;
foreground colors participate in the COLRv1 atlas cache key. Temporary glyph
atlases retain upstream's shared-borrow fix and lifecycle unchanged.

This branch stays on the Slint 1.18-compatible 0.27 release baseline
`0eb55780894a9200e9a70ca3d16bd1c641281a44`, with upstream's shared-borrow fix
`61d24855a2c3c73efaab6fe9108f642fc6569067` backported together with its regression
test. Newer upstream changes replace `GaussianBlur { sigma }` with per-axis
fields, which Slint 1.18 cannot use yet. The only additional feature is COLRv1.
Graph work, recursion, temporary storage and glyph raster dimensions are capped.
Gradient interpolation follows native Skia's sRGB behavior; see the independent
reference fixtures in `tests/fixtures/colrv1/` for the CPAL specification distinction.

Until tiny-skia releases the partial-sweep normalization fix, this repository
patches it to `misobarisic/tiny-skia` revision
`7cb203434cc55baa98333b5c480da158d08ce866`. Cargo ignores dependency-level patches,
so applications enabling `colrv1` must add the same `[patch.crates-io]` entries
for both `tiny-skia` and `tiny-skia-path` to keep shared geometry types identical.

```sh
cargo test --lib --no-default-features --features colrv1
cargo test --lib --features colrv1
# Optional exhaustive application-font corpus, without bundling it here:
COLRV1_TEST_FONT=/path/to/NotoColorEmoji.ttf cargo test --lib --features colrv1 external_color_font_rasterizes -- --ignored
```

## Screenshots
### Demo
![demo](assets/demo.png)
Run with `cargo run --example demo`

### Breakout
![breakout](assets/breakout.png)
Run with `cargo run --example breakout`

### SVG
![svg](assets/svg.png)
Run with `cargo run --example svg`

### Text
![text](assets/text.png)
Run with `cargo run --example text`

## Features
* [x] Anti-aliasing
* [x] Bézier paths filling and stroking
* [x] Solid color and image pattern fills and strokes
* [x] Gradients - box, linear and radial
* [x] Stroke width and miterlimit
* [x] Stroke caps: butt, round and square
* [x] Stroke joins: miter, round and bevel
* [x] Fill rules - EvenOdd/NonZero
* [x] Rectangle scissoring
* [x] Composition modes (SourceOver, SourceIn, SourceOut, Atop, etc..)
* [x] Global alpha
* [x] Text filling and stroking
* [x] Text shaping
* [x] Text alignment: (left center right), (top, middle, alphabetic, bottom)
* [x] Nearest font matching
* [x] Path hit testing
* [x] OpenGl ES2 backend
* [x] WGPU backend
* [x] Color fonts

## Not supported
* [ ] Stroke dashing
* [ ] Path scissoring
* [ ] Custom shaders
* [ ] 3D transforms

## Ecosystem

FemtoVG is used by these projects:

 - pn-editor <https://gitlab.com/porky11/pn-editor>:
   view, edit, and simulate petri nets
 - Slint <https://slint.dev>:
   A declarative GUI toolkit to build native user interfaces for embedded, desktop, and mobile application
 - inlottie <https://github.com/mhfan/inlottie>:
   lib/tool in Rust for Lottie (Bodymovin) Animation
 - Mapvas <https://github.com/UdHo/mapvas>:
   A map canvas showing OSM tiles with drawing functionality

## License
Licensed under either of
 * Apache License, Version 2.0 ([LICENSE-APACHE](docs/LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](docs/LICENSE-MIT) or http://opensource.org/licenses/MIT)
at your option.

Fonts used in examples:
- Roboto licensed under [Apache license](http://www.apache.org/licenses/LICENSE-2.0)
- Entypo licensed under CC BY-SA 4.0.
- Amiri licensed under [SIL Open Font License, Version 1.1](http://scripts.sil.org/cms/scripts/page.php?site_id=nrsi&id=OFL)
