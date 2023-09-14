# GameGUI Change Log

# v1.14

## Command Line Commands

- `:clamp` is now `:filter clamp`.
- `:clamph` has been removed (use `:filter clamp`).
- `:clampl` has been removed (use `:filter clamp`).
- `:hsv2rgb` is now `:filter hsvtorgb`.
- `:rgb2hsv` is now `:filter rgbtohsv`.

## Filters

### clamp

    :filter clamp <low-color> <high-color>

Limits the range of each pixel's color components to the corresponding low and high byte of the specified ARGB values. For example, `clamp 048 FCA` limits each red component to the range 00-FF, each green component to 44..CC, and each blue component to 88..AA.

### gray

    :filter gray
    :filter gray 299 587 114

- Now accepts RGB weight arguments.
- Weights are normalized. `:filter gray 1 2 1` and `:filter gray 0.25 0.50 0.25` are equivalent.

### hsvtorgb

    :filter hsvtorgb

Treats pixel RGB color components as HSV (likely from a prior `:filter rgbtohsv` conversion) and performs an HSV→RGB conversion on them.

### rgbtohsv

    :filter rgbtohsv

Converts pixel RGB color components to HSV (Hue, Saturation, Value) format. After the filter is applied, the Red component 0-255 indicates Hue 0-255, Green indicates Saturation, and Blue indicates Value. Use `:filter hsvtorgb` to perform the reverse conversion.
