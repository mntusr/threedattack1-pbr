A Threedattack1-specific fork of [panda3d-simplepbr](https://github.com/Moguri/panda3d-simplepbr).

Added features:

* Shadow blur
* Force objects to be completely shadowed or completely non-shadowed

Installation:

```bash
pip install git+https://github.com/mntusr/threedattack1-pbr.git

```

The fork is based on: [6172a825a040901fcc3edcbcd550829f3b36d6a3](https://github.com/Moguri/panda3d-simplepbr/tree/6172a825a040901fcc3edcbcd550829f3b36d6a3)

# Original readme

## panda3d-simplepbr

This is a simple, basic, lightweight, no-frills PBR render pipeline for [Panda3D](https://www.panda3d.org/).
It is currently intended to be used with [panda3d-gltf](https://github.com/Moguri/panda3d-gltf), which will output textures in the right order.
The PBR shader is heavily inspired by the [Khronos glTF Sample Viewer](https://github.com/KhronosGroup/glTF-Sample-Viewer).
*Note:* this project does not make an attempt to match a reference renderer.

### Features
* Supports running on a wide range of hardware with an easy OpenGL 2.1+ requirement
* Forward rendered metal-rough PBR
* All Panda3D light types (point, directional, spot, and ambient)
* Filmic tonemapping 
* Normal maps
* Emission maps
* Occlusion maps
* Basic shadow mapping for DirectionalLight and Spotlight
* Post-tonemapping color transform via a lookup table (LUT) texture
* IBL Diffuse
* IBL Specular

### Notable Todos
There are a few big things still missing and are planned to be implemented:

* Shadow mapping for PointLight

### Other missing features
The goal is to keep this simple and lightweight.
As such, the following missing features are *not* currently on the roadmap:

* Something to deal with many lights (e.g., deferred, forward+, tiling, clustering, etc.)
* Fancy post-process effects (temporal anti-aliasing, ambient occlusion, screen-space reflections)
* Environment probes

### <strike>Installation</strike>

<strike>Use pip to install the `panda3d-simplepbr` package:

```bash
pip install panda3d-simplepbr
```

To grab the latest development build, use:

```bash
pip install git+https://github.com/Moguri/panda3d-simplepbr.git

```
</strike>

### Usage

Just add `simplepbr.init()` to your `ShowBase` instance:

```python
from direct.showbase.ShowBase import ShowBase

import simplepbr

class App(ShowBase):
    def __init__(self):
        super().__init__()

        simplepbr.init()
```

The `init()` function will choose typical defaults, but the following can be modified via keyword arguments:

`render_node`
: The node to attach the shader too, defaults to `base.render` if `None`

`window`
: The window to attach the framebuffer too, defaults to `base.win` if `None`

`camera_node`
: The NodePath of the camera to use when rendering the scene, defaults to `base.cam` if `None`

`msaa_samples`
: The number of samples to use for multisample anti-aliasing, defaults to 4

`max_lights`
: The maximum number of lights to render, defaults to 8

`use_normal_maps`
: Use normal maps to modify fragment normals, defaults to `False` (NOTE: Requires models with appropriate tangents defined)

 `use_emission_maps`
: Use emission maps, defaults to `True`

`use_occlusion_maps`
: Use occlusion maps, defaults to `False` (NOTE: Requires occlusion channel in metal-roughness map)

`enable_shadows`
: Enable shadow map support, defaults to `True`

`enable_fog`
: Enable exponential fog, defaults to False

`exposure`
: a value used to multiply the screen-space color value prior to tonemapping, defaults to 1.0

`use_330`
: Force shaders to use GLSL version 330 (if `True`) or 120 (if `False`) or auto-detect if `None`, defaults to `None`

`use_hardware_skinning`
: Force usage of hardware skinning for skeleton animations or auto-detect if `None`, defaults to `None`

`sdr_lut`
: Color LUT to use post-tonemapping

`sdr_lut_factor`
: Factor (from 0.0 to 1.0) for how much of the LUT color to mix in, defaults to 1.0

Those parameters can also be modified later on by setting the related attribute of the simplepbr pipeline returned by the init() function:

```python
        pipeline = simplepbr.init()
        
        ...
        
        pipeline.use_normals_map = True
```

#### Textures

simplepbr expects the following textures are assigned to the following texture stages:

* BaseColor - Modulate
* MetalRoughness - Selector
* Normals - Normal
* Emission - Emission

#### Example

For an example application using `panda3d-simplepbr` check out the [viewer](https://github.com/Moguri/panda3d-gltf/blob/master/gltf/viewer.py) in the [panda3d-gltf repo](https://github.com/Moguri/panda3d-gltf).

### Running Tests

First install `panda3d-simplepbr` in editable mode along with `test` extras:

```bash
pip install -e .[test]
```

Then run the test suite with `pytest`:

```bash
pytest
```

### Building Wheels

Install `build`:

```bash
pip install --upgrade build
```

and run:

```bash
python -m build
```

### License
[B3D 3-Clause](https://choosealicense.com/licenses/bsd-3-clause/)
