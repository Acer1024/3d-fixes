Unity 5 Templates
=================

These are a collection of templates that can be used as a starting point to fix
any Unity game. Either DX9 or DX9-New can be used for DX9 games - check below
for a few key differences between the two and reasons you might select one over
the other.

Don't forget, in some Unity games it is possible to force DX9 or DX11 mode with
the -force-d3d9 and -force-d3d11 launch options, so you may be able to choose
any of these templates. If you do this, be sure to check that the game still
renders correctly - some games will work perfectly, others may be missing a few
effects (that may be minor or serious), and others won't work at all. This can
happen in either direction - just because DX11 can theoretically do everything
that DX9 can doesn't mean a DX9 Unity game will work in DX11 (case in point:
The Forest, Dreamfall Chapters).

DX9-New
-------
As the name implies, this is the newer way to fix Unity games. It works by
applying a world-space correction to lighting shaders and every shader that
uses the world space camera position.

Advantages:

- Accurate fix for all specular highlights and reflections

- Also pops certain parallax shaders into 3D (e.g. Rocks + sand in The Forest)

- Required matrices used by the fix are more widly available than those used in
  the old style fix

- Compatible with various other fixes that need world-space adjustments (yay, I
  can finally fix the light shafts in The Forest!)

- Adds directives to copy the depth buffer to the DX9Settings.ini to make it
  easier to add an auto-UI adjustment later (Note that this does not add a UI
  adjustment as that is very game-specific, it just adds the depth buffer
  copy).

- Works with Unity 5.3

Disadvantages:

- May need to find additional sources of the required matrices (but there is a
  script provided to do this for you)

- Adjusts a *lot* of shaders.

- Includes a *lot* of matrix copy directives in the DX9Settings.ini, which can
  make it hard to track down a single bad source if things are broken.

- If matrix copying is not working for whatever reason this variant will not
  work at all.

- Potentiallly incompatible with various other fixes that need view-space
  adjustments (I say potentially as there are some free constant register copy
  slots, so if directional lighting is enabled it may still be possible, plus
  depending on the effect a hard-coded FOV value may be acceptable)

- If the required matrices are not copied, all lighting will be broken,
  including directional lighting (I should be able to improve the directional
  case though)

- Still needs some minor updates for Unity 5.4

DX9
---
This template works by applying a view-space correction to the lighting pixel
shaders.

Advantages:

- Simpler fix, as fewer shaders need to be included.

- Directional lighting is guaranteed to be fixed.

- Fewer matrix copy directives to be suspicious of if things aren't working.

- If matrix copying is not working for whatever reason the FOV can be hardcoded
  as a workaround.

- Compatible with various other fixes that need view-space adjustments.

Disadvantages:

- May need to find additional sources of the MV + MVP matrices to copy, which
  can be rather tricky.

- The required matrices may not be available at all.

- Will only accurately fix specular highlights and reflections that were drawn
  by the deferred physical lighting shaders. Other specular highlights and
  reflections drawn by the surface shaders will not be fixed (this may include
  reflections on water).

- Incompatible with various other fixes that need world-space adjustments
  (unless they are self-contained and do not require additional matrices to be
  copied/inverted with Helix Mod)

- Does not work with Unity 5.3, but could be adapted to do so fairly easily.

Alternate DX9 Fix
-----------------
While not included in the templates, there is a third way to fix Unity lights
that requires no matrices whatsoever, and instead uses the camera frustrum and
clipping planes to calculate the FOV for a view-space correction. Refer to the
fix for The Last Tinker: City of Colors for this technique. This technique is
unlikely to work in many games due to the fact that the camera frustrum is even
rarer than the model-view matrix in many Unity games.

DX11
----
This template is mostly on parity with the DX9-New template. It applies a
view-space correction to the lighting pixel shades and a world-space correction
to the camera position everywhere else.

It includes lighting vertex shaders and a script to fix lighting pixel shaders.

Advantages

- Avoids the limitations of Helix Mod, nicer environment to work in, more
  features possible in the future thanks to 3DMigoto.

- Accurate fix for all specular highlights and reflections

- Also pops certain parallax shaders into 3D (e.g. Rocks + sand in The Forest)

- Able to use multiple matrices for both view and world space corrections.

- Adds directives to copy the depth buffer to the DX9Settings.ini to make it
  easier to add an auto-UI adjustment later (Note that this does not add a UI
  adjustment as that is very game-specific, it just adds the depth buffer
  copy).

- Directional, point and spot lights should always be fixed.

- Works with Unity 5.3 and 5.4 games.

Disadvantages:

- Subject to 3DMigoto decompiler bugs. The script uses fxc to verify that the
  shaders will at least compile and renames any that fail, but it cannot detect
  subtle bugs the decompiler may introduce. In practice, Unity does not trigger
  many decompile bugs so this may not be a significant issue.

- If directional lighting is not enabled, physical lighting may be broken as it
  currently lacks the fallbacks that the DX9 version has.

- Adjusts a *lot* of shaders.

- Includes a *lot* of matrix copy directives in the DX9Settings.ini, which can
  make it hard to track down a single bad source if things are broken.
