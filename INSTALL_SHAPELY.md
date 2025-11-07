# Installing Shapely for Blender

TexCut requires Shapely for HIGH quality mesh generation (quality=0.001). Without Shapely, you can only use LOW and MEDIUM quality presets.

## Why Shapely is Required

HIGH quality mode produces very detailed meshes (~160 vertices) but can create self-intersecting polygons. Shapely's `buffer(0)` method is the industry-standard technique for fixing invalid geometry while preserving maximum detail.

Without Shapely, the add-on will show an error when attempting to use HIGH quality.

## Installation Methods

### Method 1: Blender Python Console (Recommended)

1. Open Blender
2. Switch to the Scripting workspace
3. In the Python Console, run:

```python
import subprocess
import sys

# Get Blender's Python executable
python_exe = sys.executable

# Install Shapely
subprocess.check_call([python_exe, "-m", "pip", "install", "shapely"])

print("Shapely installed successfully!")
```

4. Restart Blender

### Method 2: Command Line (macOS)

```bash
# Blender 4.x
/Applications/Blender.app/Contents/Resources/4.0/python/bin/python3.11 -m pip install shapely

# Blender 3.x
/Applications/Blender.app/Contents/Resources/3.6/python/bin/python3.11 -m pip install shapely
```

### Method 3: Command Line (Windows)

```cmd
:: Blender 4.x
"C:\Program Files\Blender Foundation\Blender 4.0\4.0\python\bin\python.exe" -m pip install shapely

:: Blender 3.x
"C:\Program Files\Blender Foundation\Blender 3.6\3.6\python\bin\python.exe" -m pip install shapely
```

### Method 4: Command Line (Linux)

```bash
# Blender 4.x
/usr/share/blender/4.0/python/bin/python3.11 -m pip install shapely

# Blender 3.x
/usr/share/blender/3.6/python/bin/python3.11 -m pip install shapely
```

## Verification

To verify Shapely is installed correctly:

1. Open Blender
2. Go to Scripting workspace
3. In Python Console, run:

```python
import shapely
print(f"Shapely version: {shapely.__version__}")
```

If you see the version number, Shapely is installed correctly!

## Troubleshooting

### "No module named 'shapely'"

This means Shapely is not installed. Follow one of the installation methods above.

### "Permission denied" or "Access denied"

On macOS/Linux, try:
```bash
sudo /Applications/Blender.app/Contents/Resources/4.0/python/bin/python3.11 -m pip install shapely
```

On Windows, run the Command Prompt as Administrator.

### "pip not found"

Install pip first:
```bash
# macOS
/Applications/Blender.app/Contents/Resources/4.0/python/bin/python3.11 -m ensurepip

# Windows
"C:\Program Files\Blender Foundation\Blender 4.0\4.0\python\bin\python.exe" -m ensurepip
```

Then retry the Shapely installation.

## Alternative: Use MEDIUM Quality

If you cannot install Shapely, you can still use TexCut with LOW and MEDIUM quality presets:

- **LOW** (quality=0.02): ~7 vertices, convex hull, fastest
- **MEDIUM** (quality=0.01): ~10 vertices, convex hull, good balance

These presets don't require Shapely and work out of the box.

## Technical Details

- **Shapely version**: 2.0+ recommended
- **Dependencies**: Shapely automatically installs numpy (usually already in Blender)
- **Size**: ~2-3 MB installed
- **License**: BSD 3-Clause (compatible with commercial use)

## Benefits of Installing Shapely

| Feature | Without Shapely | With Shapely |
|---------|----------------|--------------|
| HIGH quality available | ❌ No | ✅ Yes |
| Maximum vertex detail | 10 vertices | 160 vertices |
| Concave features | Basic | Fully preserved |
| Self-intersections | N/A | Automatically fixed |
| Installation | None needed | One-time setup |

**Recommendation**: Install Shapely for best results with complex shapes like trees, characters, or detailed sprites.
