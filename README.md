CUDA Path Tracer
================
**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 3**

* Anirudh Akula
* Tested on: Windows 11, NVIDIA T1000 4096MB (CETS Virtual PC)



## Features
- bullets
  
## Quick Start
- how to build and run 

## Theory
- path tracing loop (bouncing)
- camera ray gen
- intersections and testing (Moller trumbore)

## Representations
- Rays, Geoms and materials
- mesh to GPU buffered triangles
<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/58efed41-c463-4213-8859-fc6648a93f2b" />

## Optimizations
- show stats and charts
- culling
- octotree
- stream compaction
- material sorting

## Visuals
- show AA
- show different materials rendered

## Credits

### Libraries

- **[tinygltf](https://github.com/syoyo/tinygltf)** Used for glTF 2.0 mesh loading (`.gltf` / `.glb`).  
  Licensed under the [MIT License](https://opensource.org/licenses/MIT).

### 3D Models

Models sourced from the [Khronos glTF Sample Assets](https://github.com/KhronosGroup/glTF-Sample-Assets) repository:

| Model | Asset | Notes / License |
|---|---|---|
| **Duck** | [Duck](https://github.com/KhronosGroup/glTF-Sample-Assets/tree/main/Models/Duck) | Classic glTF sample duck (`Duck.glb`) |
| **Dragon** | [DragonAttenuation](https://github.com/KhronosGroup/glTF-Sample-Assets/tree/main/Models/DragonAttenuation) | Stanford dragon mesh ([Stanford Graphics Library](http://www.graphics.stanford.edu/data/3Dscanrep/)); based on [Morgan McGuire’s Computer Graphics Archive](https://casual-effects.com/data). Cloth backdrop: [CC0](https://creativecommons.org/publicdomain/zero/1.0/) |
| **Skull** | [ScatteringSkull](https://github.com/KhronosGroup/glTF-Sample-Assets/tree/main/Models/ScatteringSkull) | Model files licensed [CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/) |

Khronos sample-asset metadata / documentation is typically under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/).

