CUDA Path Tracer
================
**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 3**

* Anirudh Akula
* Tested on: Windows 11, NVIDIA T1000 4096MB (CETS Virtual PC)

<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/cbff80bb-2f2e-436b-8f2b-5421c572a762" />

<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/9284b49c-2e99-4d8b-ac84-b1b767dd4ea1" />


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

| Model | Asset |
|---|---|
| **Duck** | [Duck](https://github.com/KhronosGroup/glTF-Sample-Assets/tree/main/Models/Duck) |
| **Dragon** | [DragonAttenuation](https://github.com/KhronosGroup/glTF-Sample-Assets/tree/main/Models/DragonAttenuation) |
| **Skull** | [ScatteringSkull](https://github.com/KhronosGroup/glTF-Sample-Assets/tree/main/Models/ScatteringSkull) |

Used AI agents to generate JSON scene files (Model: Claude Opus 5.5)
