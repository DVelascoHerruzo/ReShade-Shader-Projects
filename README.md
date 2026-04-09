# ReShade Shader Projects

Personal ReShade shader projects meant for practice, heavily inspired by and reverse-engineered from [AcerolaFX](https://github.com/GarrettGunnell/AcerolaFX).

---

## Installation

1. **Install ReShade** in your target game by running the [ReShade installer](https://reshade.me) and selecting the game's executable.
2. **Copy shader files** — place the contents of the `Barope Shaders/` folder into your game's ReShade shader directory:
   - `.fx` files → `reshade-shaders/Shaders/`
   - `Includes/*.fxh` files → `reshade-shaders/Shaders/Includes/`
3. **Launch the game** and open the ReShade overlay (default key: `Home`).
4. **Enable shaders** — go to the *Home* tab in the ReShade overlay, click the checkbox next to the shader(s) you want to use, and adjust settings as needed.

> **Note:** Some shaders (`ASCII.fx`) use depth and normal buffers. Make sure **"Copy depth buffer before clear operations"** is enabled in the ReShade *Settings* tab for correct results.

---

## Shaders

### `ASCII.fx`
Transforms the screen into ASCII art. Edge detection is performed using a Difference of Gaussians (DoG) filter, optionally informed by depth and surface normals, and the result is mapped to ASCII characters driven by pixel luminance.

**Settings:**
- **Preprocess Settings** — Control zoom/offset, Gaussian blur kernel and sigma, DoG detail and threshold, depth/normal-based edge detection and cutoffs, and edge fill threshold.
- **Color Settings** — Toggle edge and fill drawing independently, adjust luminance exposure and attenuation, invert the luminance-to-character mapping, set ASCII and background colors, blend with the original render color, and control depth-based character fade-out.

---

### `PaletteSwap.fx`
Reduces the screen to a restricted color palette. Palette colors are matched per-pixel in the OKLAB perceptual color space for visually accurate quantization.

**Settings:**
- **Manual Colors** — Directly define up to 8 palette colors (controlled by the `BFX_PALETTE_COUNT` preprocessor define, default: 4).
- **Random Palette Settings** — Generate a palette procedurally via a seed and color count (3–16 colors). Hue arrangement is selectable (Monochromatic, Analogous, Complementary, Triadic, Tetradic). Luminance, chroma, and their per-color variance ranges are independently controllable.

---

## Includes

These header files are shared dependencies required by the shaders above. They do not need to be enabled in ReShade.

| File | Description |
|------|-------------|
| `BaropeFX_Common.fxh` | Shared utilities: common full-resolution textures/samplers (point, linear, mirror, wrap, border), helper functions (`Luminance`, `Map`, `WhiteBalance`), and color space conversion matrices (sRGB ↔ XYZ ↔ OKLAB). |
| `BaropeFX_TempTex1.fxh` | Temporary RGBA16F render target (`BFX_RenderTex1`) used as an intermediate pass buffer. Provides point, linear, and compute storage samplers. |
| `BaropeFX_TempTex2.fxh` | Same as `TempTex1` but declares a second independent render target (`BFX_RenderTex2`) to avoid read/write conflicts in multi-pass effects. |
| `BaropeFX_TempTex3.fxh` | Third intermediate render target (`BFX_RenderTex3`) for shaders requiring more than two passes. |
| `BaropeFX_TempTex4.fxh` | Fourth intermediate render target (`BFX_RenderTex4`) for the most pass-heavy effects. |
