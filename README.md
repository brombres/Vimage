# Vimage

Summary   | Current Release
----------|-----------------------
Version   | 1.1
Date      | April 9, 2023
Platforms | Windows, macOS, Linux
License   | [MIT License](LICENSE)
Author    | Brom Bresenham


# About
Vimage is a console-based image manipulator with a Vim-style user interface.

Vimage uses character block graphics to display and edit JPEGs, PNGs, and BMPs in 24-bit truecolor where possible. Windows and Linux consoles support truecolor out of the box. "Terminal" built in to macOS only supports an 8-bit color display but iTerm2 on macOS supports truecolor.

Vimage focuses on general image manipulations such as resizing, cropping, flipping, rotating, splitting, and joining. It does not currently offer pixel-level editing.

# Installation

## New Installation

1. Install [morlock.sh](https://morlock.sh)
2. `morlock install brombres/vimage`

## Updating Existing Installation
`morlock update shellview`

# Usage

    vimage

    vimage paths/to/files/and/folders

# Key Commands

Precede a key command with a number to repeat the command that many times or apply the command to that many images, as appropriate - for example, `5n` horizontally joins 5 images, starting with the current image.

Key    | Command              | Description
-------|----------------------|----------------------------------------------------------------------
`a`    | Apply Macro          | Applies the current command macro (recorded with `q`) to the current image.
`A`    | Apply Macro To All   | Applies the current command macro (recorded with `q`) to the current image and all images below.
`c`    | Crop Mode            | Enters Crop Mode. `[h,j,k,l]` crop 1 pixel off the left/bottom/top/right, respectively. `[H,J,K,L]` expands by one pixel, using the `:bg` color to fill the gap. `100j` crops 100 pixels off the bottom, etc. Tap `ESCAPE` to exit Crop Mode.
`d`    | Delete/Cut           | Deletes the current image and copies it to the clipboard.
`f`    | Flip Horizontal      | Flips (mirrors) the current image horizontally.
`F`    | Flip Vertical        | Flips (mirrors) the current image vertically.
`g`    | Go to First Image    | Selects the first image in the image list.
`G`    | Go to Last Image     | Selects the last image in the image list.
`j`,`k`| Next/Previous Image  | Selects the next or previous image in the Image List to view or edit.
`J`,`K`| Shift List Position  | Shifts the current image up or down in the Image List, reordering the list.
`n`    | Join Horizontal      | Joins (splices) the current image with the one below it, placing them side-by-side horizontally.
`N`    | Join Vertical        | Joins (splices) the current image with the one below it, stacking them vertically.
`p`    | Paste Above          | Pastes the clipboard image above the current image in the Image List.
`P`    | Paste Below          | Pastes the clipboard image below the current image in the Image List.
`q`    | Record Macro         | Tap `q` to begin recording a macro, enter any commands desired, and then tap `q` again to stop recording the macro. Use `a` to replay the current macro on the currently-selected image.
`r`    | Rotate Clockwise 90º | Rotates the current image clockwise by 90º.
`R`    | Rotate CCW 90º       | Rotates the current image counter-clockwise by 90º.
`t`    | Stack                | Stacks (merges) the current image on the one below it using alpha blending. Use e.g. `4t` to stack the current image and 3 images below it into a single image.
`u`    | Undo                 | Undo the last editing command (the most recent 50 commands can be undone).
`U`    | Redo                 | Redo the last editing command that was just undone. Can also use `CONTROL+R` to Redo.
`y`    | Yank/Copy            | Yank a copy of the current image into the clipboard.
`z`    | Zoom In              | Enter Zoom Mode and zoom in to the current image. Use `[i,j,k,l]` to pan around. Use `Z` to zoom out and `ESCAPE` to exit Zoom Mode.
`Z`    | Zoom Out             | Enter Zoom Mode and zoom out of the current image. Use `[i,j,k,l]` to pan around. Use `z` to zoom in and `ESCAPE` to exit Zoom Mode.
`0`    | Default Zoom         | Resets the current image zoom level to the default, which fits the image to the window.
`.`    | Repeat Last          | Repeats the last editing command.
`:`    | Command Line         | Begins input of a command line (see [Command Line Commands](#Command-Line-Commands)).
`"`    | Toggle Clipboard     | Toggles between using the OS clipboard (default) and the internal clipboard for operations `d` (delete/cut), `y` (yank/copy), and `p` (paste). The OS clipboard allows image copy/paste between Vimage and other apps but can be slower when dealing with large images.
`!`    | Bitwise NOT          | Inverts the RGB pixel values of the current image.
`&`    | Bitwise AND          | Combine this image with the image below using a bitwise AND operation.
`\|`    | Bitwise OR           | Combine this image with the image below using a bitwise OR operation.
`~`    | Bitwise XOR          | Combine this image with the image below using a bitwise XOR operation.
`<`    | Bitwise ROL          | Rotate the RGB pixel bit values left by 1 bit. Use e.g. `8<` to ROL 8 bits, etc.
`>`    | Bitwise ROR          | Rotate the RGB pixel bit values right by 1 bit. Use e.g. `8>` to ROR 8 bits, etc.

# Command Line Commands
Type `:` to begin a Command Line command. Type one of the following and then tap `ENTER` to execute or `ESCAPE` to cancel.

Command   | Arguments       | Description
----------|-----------------|----------------------------------------------------------------------
`:anchor`   | `[center\|top\|left\|bottom\|right]+`<br>`[c\|t\|l\|b\|r]+` | Sets the anchor used when when an image is cropped or expanded.
`:aspect`   | `[16:9\|3:4\|...]`  | Crops or expands the image as necessary to achieve the specified aspect ratio.
`:bg`       | `[V\|RGB\|ARGB\|RRGGBB\|AARRGGBB]`<br>`[transparent\|white\|red\|...\|random]` | Sets the background color used for `:new` images and for expanded areas of an existing image.
`:clamp`    | `LOW HIGH`        | Limits the range each pixel's color components to the corresponding low and high of the specified ARGB values. For example, `clamp 048 FCA` limits each red component to the range 00-FF, each green component to 44..CC, and each blue component to 88..AA.
`:clamph`   | `HIGH`        | Equivalent to `:clamp 0000 HIGH`.
`:clampl`   | `LOW`         | Equivalent to `:clamp LOW ffff`.
`:copy`     | `[a\|r\|g\|b\|XX]` `[a\|r\|g\|b]+` | Copy the specified source channel (a,r,g,b) or a constant channel value (such as `7f`) to one or more destination channels. To copy red to green and blue channels: `:copy r gb`. See also: `:swap`.
`:crop`     | `WxH`             | Crop/expand to size WxH (e.g. `:crop 640x480`). If W and H are real numbers then they are treated as proportions - `:crop 0.5x2.0` crops half the width and expands to twice the height.
`:crop`     | `Wx`              | Crop/expand to width W, preserving the current height.
`:crop`     | `xH`              | Crop/expand to height H, preserving the current width.
`:crop`     | `N`               | Crop N pixels on all sides. If N is negative then sides are expanded instead.
`:crop`     | `LEFT TOP RIGHT BOTTOM` | Crop/expand the specified number of pixels on each side. `:crop 1 -2 0 1` crops the left and bottom by 1 pixel and expands the top by 2 pixels.
`:fill`     | `[WxH\|Wx\|xH]`     | Crops, expands, and/or resizes the current image to aspect-fill the specified size, retaining the same aspect ratio while completely filling the specified dimensions. Some content may be lost.
`:fit`      | `[WxH\|Wx\|xH]`     | Crops, expands, and/or resizes the current image to aspect-fit the specified size, retaining the same aspect ratio while fitting all content within the specified dimensions. The result may be letterboxed or pillarboxed.
`:gradient` | `LEFT RIGHT`<br>`TL TR BL BR` | Fills the current image with a color gradient using either 2 side colors or 4 corner colors.
`:ha`       |                 | Toggle "hard alpha" view on or off. When hard alpha is on, any pixel that is not completely transparent is shown as completely opaque. This helps ensure that translucent pixels are not accidentally cropped off.
`:hsb2rgb`  |                 | The corollary to `:rgb2hsb`, this assumes the current image contains HSB color components and converts them to RGB color components.
`:join`     | `WxH`             | Join or splice the next W\*H images together in a WxH grid. Images are aspect-fit-resized as necessary so that all grid cells are a consistent size.
`:&`        | `[V\|RGB\|ARGB\|RRGGBB\|AARRGGBB]` | Bitwise-AND's each pixel in the current image with the specified constant value.
`:\|`       | `[V\|RGB\|ARGB\|RRGGBB\|AARRGGBB]` | Bitwise-OR's each pixel in the current image with the specified constant value.
`:~`        | `[V\|RGB\|ARGB\|RRGGBB\|AARRGGBB]` | Bitwise-XOR's each pixel in the current image with the specified constant value.
`:<`        | `N`               | Left-rotates the RGB bits by N pixels. Does not affect the alpha bits. Example: `:< 8`.
`:<`        | `N a`             | Left-rotates the ARGB bits by N pixels. Example: `:< 8 a`.
`:>`        | `N`               | Right-rotates the RGB bits by N pixels. Does not affect the alpha bits. Example: `:> 8`.
`:>`        | `N a`             | Right-rotates the ARGB bits by N pixels. Example: `:> 8 a`.
`:new`      | `WxH`             | Creates a new image of the specified pixel size.
`:open`     | `<filepaths>`     | Opens one or more specified images and adds them to the Image List.
`:q`        |                 | Quits (closes) the current image. Will be unsuccessful if the image has modifications.
`:q!`       |                 | Quits (closes) the current image, even if it has modifications.
`:qq`       |                 | Quits all images and ends Vimage. Will be unsuccessful if any image has modifications.
`:qq!`      |                 | Quits all images, even if some of them have modifications.
`:rgb2hsb`  |                 | Converts the current image's RGB color components into HSB (Hue Saturation Brightness, also called HSV or Hue Saturation Value) color components. Each pixel's "red" value now indicates hue (0-255), "green" is saturation (0-255), and "blue" is brightness (0-255). These channels can be further manipulated using commands such as `:&`, `:|`, and `:clamp`. Then the image can be converted back into RGB with the command `:hsb2rgb`.
`:rm`       |                 | Removes (deletes) the file for the current image from the filesystem. Useful for cleaning up an image folder by deleting unwanted images.
`:resize`   | `[WxH\|Wx\|xH]`     | Resizes the current image. "Wx" and "xH" variations retain the current aspect ratio while specifying a new size for one dimension.
`:save`     | `[filepath]`      | An alias for `:w`.
`:split`    | `[WxH]`           | Splits the current image into W\*H images. For example, `:split 4x2` assumes the current image is 4 tiles wide and 2 tiles high and splits the image into 8 separate tile images.
`:swap`     | `[a\|r\|g\|b]` `[a\|r\|g\|b]` | Swaps one color channel with another in the current image. For example, `:swap r b` swaps the red and blue channels.
`:w`        | `[filepath]`      | Writes (saves) the current image, optionally specifying a new filepath.
`:wall`     |                   | Writes (saves) all modified images.
`:wq`       |                   | Writes (saves) and quits (closes) the current image.
