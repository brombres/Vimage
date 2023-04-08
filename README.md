# Vimage

Summary   | Current Release
----------|-----------------------
Version   | 0.1
Date      | February 19, 2023
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

Key  | Command              | Description
-----|----------------------|----------------------------------------------------------------------
a    | Apply Macro          | Applies the current command macro (recorded with `q`) to the current image.
A    | Apply Macro To All   | Applies the current command macro (recorded with `q`) to the current image and all images below.
c    | Crop Mode            | Enters Crop Mode. `[h,j,k,l]` crop 1 pixel off the left/bottom/top/right, respectively. `[H,J,K,L]` expands by one pixel, using the `:bg` color to fill the gap. `100j` crops 100 pixels off the bottom, etc. Tap `ESCAPE` to exit Crop Mode.
d    | Delete/Cut           | Deletes the current image and copies it to the clipboard.
f    | Flip Horizontal      | Flips (mirrors) the current image horizontally.
F    | Flip Vertical        | Flips (mirrors) the current image vertically.
g    | Go to First Image    | Selects the first image in the image list.
G    | Go to Last Image     | Selects the last image in the image list.
j, k | Next/Previous Image  | Selects the next or previous image in the Image List to view or edit.
J, K | Shift List Position  | Shifts the current image up or down in the Image List, reordering the list.
n    | Join Horizontal      | Joins (splices) the current image with the one below it, placing them side-by-side horizontally.
N    | Join Vertical        | Joins (splices) the current image with the one below it, stacking them vertically.
p    | Paste Above          | Pastes the clipboard image above the current image in the Image List.
P    | Paste Below          | Pastes the clipboard image below the current image in the Image List.
q    | Record Macro         | Tap `q` to begin recording a macro, enter any commands desired, and then tap `q` again to stop recording the macro. Use `a` to replay the current macro on the currently-selected image.
r    | Rotate Clockwise 90º | Rotates the current image clockwise by 90º.
R    | Rotate CCW 90º       | Rotates the current image counter-clockwise by 90º.
t    | Stack                | Stacks (merges) the current image on the one below it using alpha blending. Use e.g. `4t` to stack the current image and 3 images below it into a single image.
u    | Undo                 | Undo the last editing command (can be repeated).
U    | Redo                 | Redo the last editing command that was just undone. Can also use `CONTROL+R` to Redo.
y    | Yank/Copy            | Yank a copy of the current image into the clipboard.
z    | Zoom In              | Enter Zoom Mode and zoom in to the current image. Use `[i,j,k,l]` to pan around. Use `Z` to zoom out and `ESCAPE` to exit Zoom Mode.
Z    | Zoom Out             | Enter Zoom Mode and zoom out of the current image. Use `[i,j,k,l]` to pan around. Use `z` to zoom in and `ESCAPE` to exit Zoom Mode.
.    | Repeat Last          | Repeats the last editing command.
:    | Command Line         | Begins input of a command line (see [Command Line Commands](#Command-Line-Commands).
"    | Toggle Clipboard     | Toggles between using the internal clipboard and the OS clipboard for operations `d` (delete/cut), `y` (yank/copy), and `p` (paste). The internal clipboard is faster but the OS clipboard allows image copy/paste between Vimage and other apps.
!    | Bitwise NOT          | Inverts the RGB pixel values of the current image.
&    | Bitwise AND          | Combine this image with the image below using a bitwise AND operation.
|    | Bitwise OR           | Combine this image with the image below using a bitwise OR operation.
~    | Bitwise XOR          | Combine this image with the image below using a bitwise XOR operation.
<    | Bitwise ROL          | Rotate the RGB pixel bit values left by 1 bit. Use e.g. `8<` to ROL 8 bits, etc.
>    | Bitwise ROR          | Rotate the RGB pixel bit values right by 1 bit. Use e.g. `8>` to ROR 8 bits, etc.

# Command Line Commands
Type `:` to begin a Command Line command. Type one of the following and then tap `ENTER` to execute or `ESCAPE` to cancel.

Command   | Arguments       | Description
----------|-----------------|----------------------------------------------------------------------
:anchor   | [center|top|left|bottom|right]+<br>[c|t|l|b|r]+ | Sets the anchor used when when an image is cropped or expanded.
:aspect   | [16:9|3:4|...]  | Crops or expands the image as necessary to achieve the specified aspect ratio.
:bg       | [V|RGB|ARGB|RRGGBB|AARRGGBB]<br>[transparent|white|black|red|green|...] | Sets the background color used for `:new` images and for expanded areas of an existing image.
:copy     | [a|r|g|b|XX] [a|r|g|b]+ | Copy the specified source channel (a,r,g,b) or a constant channel value (such as `7f`) to one or more destination channels. To copy red to green and blue channels: `:copy r gb`.
