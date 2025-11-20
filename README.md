# TexCut - Blender Add-on

TexCut creates optimized 2D meshes from images with transparency by following the outline of non-transparent pixels. This significantly reduces pixel overdraw compared to standard quad meshes, making it ideal for scenes with lots of overlapping alpha-masked objects like forests, grass, and foliage.

## Compatibility

**Blender Version:** 4.0 and above

This addon is compatible with Blender 4.x including the latest versions (4.0, 4.1, 4.2, 4.3+). It automatically detects your Blender version and uses the appropriate API:
- **Blender 4.0-4.1**: Uses legacy blend_method and shadow_method properties
- **Blender 4.2+**: Uses the new EEVEE Next surface_render_method

For Blender 3.x users, please use version 2.2.0 of this addon.

## Features

- Analyzes image alpha channel to detect non-transparent regions
- Generates mesh geometry that follows the transparency outline
- Automatically applies the image as a texture
- Maintains image aspect ratio
- User-adjustable boundary offset (prevents edge clipping)

## Examples

### Leaf Sprite
![Leaf Example](screenshots/Example1-Leaf.png)
*Optimized mesh following the leaf outline, eliminating transparent pixels*

### Tree Billboard
![Tree Example](screenshots/Example2-Tree.png)
*Complex tree shape with efficient geometry matching the visible foliage*

## Installation

### Step 1: Install OpenCV (One-Time Setup)

This addon requires OpenCV, which is not included with Blender by default. Installation is simple:

**On Windows:**
1. Open Command Prompt
2. Run this command (replace 4.x with your Blender version, e.g., 4.5):
   ```
   mkdir "%APPDATA%\Blender Foundation\Blender\4.x\scripts\addons\modules"
   "C:\Program Files\Blender Foundation\Blender 4.x\4.x\python\bin\python.exe" -m pip install --target="%APPDATA%\Blender Foundation\Blender\4.x\scripts\addons\modules" opencv-python
   ```
3. Restart Blender after installation

**On macOS:**
1. Open Terminal
2. Run this command (replace 4.x with your Blender version, e.g., 4.5):
   ```
   mkdir -p "$HOME/Library/Application Support/Blender/4.x/scripts/addons/modules"
   /Applications/Blender.app/Contents/Resources/4.x/python/bin/python3.11 -m pip install --target="$HOME/Library/Application Support/Blender/4.x/scripts/addons/modules" opencv-python
   ```
3. Restart Blender after installation

**On Linux:**
1. Open Terminal
2. Run this command (replace 4.x with your Blender version, e.g., 4.5):
   ```
   mkdir -p "$HOME/.config/blender/4.x/scripts/addons/modules"
   /usr/share/blender/4.x/python/bin/python3.11 -m pip install --target="$HOME/.config/blender/4.x/scripts/addons/modules" opencv-python
   ```
   Note: Blender Python path may vary by installation (could be in `/usr/bin/`, `/opt/blender/`, etc.)
3. Restart Blender after installation

### Step 2: Install the Add-on

1. Download the latest release zip from [Releases](https://github.com/rawnsley/TexCut/releases)
2. In Blender, go to `Edit > Preferences > Add-ons`
3. Click `Install...` and select the downloaded zip file
4. Enable the add-on by checking the checkbox next to "Image: TexCut"

## Requirements

This add-on uses these Python packages:
- `numpy` ✅ (included with Blender)
- `opencv-python` ⚠️ **Requires installation** (see installation instructions above)

Only OpenCV needs to be installed - it's a simple one-time setup using pip.

## Usage

1. Open an image in Blender's **Image Editor**
2. In the Image Editor sidebar (press `N` to toggle), find the **TexCut** panel
3. Click **"Create Mesh from Image"**
4. Adjust settings in the dialog:
   - **Alpha Threshold**: How transparent a pixel needs to be to ignore (default: 0.01)
   - **Boundary Offset**: Extra pixels around the edge to prevent clipping (default: 8px, minimum: 1px)
5. Click **OK**

The add-on will:
- Analyze the alpha channel
- Generate an optimized mesh following the non-transparent regions
- Apply the image as a texture with proper alpha blending
- Scale the mesh to maintain the image's aspect ratio
- Add the mesh to your scene

## Benefits

### Reduced Pixel Overdraw
Standard quad meshes render all pixels, including fully transparent ones. TexCut's outline-based meshes only render visible pixels, which can significantly improve performance in scenes with:
- Dense forests and vegetation
- Overlapping foliage cards
- Particle systems with alpha-masked textures
- Billboard sprites

### Performance Impact
In scenes with many overlapping transparent objects, reducing overdraw can lead to:
- Faster rendering times
- Better GPU performance
- Reduced memory bandwidth usage

## Technical Details

The add-on works by:
1. Loading the image and extracting the alpha channel
2. Creating a binary mask based on an alpha threshold
3. Dilating the mask by N pixels (boundary offset) to prevent edge clipping
4. Detecting contours using OpenCV
6. Creating mesh geometry as an n-gon face
7. Setting up UV coordinates for proper texture mapping
8. Configuring shader nodes with alpha clipping

### Why Dilation?
The boundary offset dilation serves two purposes:
1. **Prevents edge clipping** - Ensures texture edges aren't cut off
2. **Reduces self-intersections** - Smooths the boundary to create valid polygons

Testing showed that 1+ pixel dilation prevents 100% of self-intersections across all test images.

## Version History

See [CHANGELOG.md](CHANGELOG.md) for detailed version history.

**Current Version: 2.3.1**
- Removed Pillow (PIL) dependency - now only requires OpenCV
- Simpler installation with fewer dependencies

**Version 2.3.0**
- Blender 4.x compatibility (4.0, 4.1, 4.2, 4.3+)
- Automatic API detection for different Blender versions
- Support for new EEVEE Next rendering system (Blender 4.2+)
- Updated installation instructions for Blender 4.x

**Version 2.2.0**
- Removed Shapely dependency (only OpenCV required)
- Minimum boundary offset: 1 pixel (prevents self-intersections)
- Simpler installation
- Smaller package size

## License

This project is licensed under the GNU General Public License v3.0 (GPL-3.0).

You are free to use, modify, and distribute this software under the terms of the GPL-3.0 license. See the [GNU GPL-3.0 License](https://www.gnu.org/licenses/gpl-3.0.html) for details.

## Contributing

Contributions, bug reports, and feature requests are welcome!