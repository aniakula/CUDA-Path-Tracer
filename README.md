CUDA Path Tracer
================
**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 3**

* Anirudh Akula
* Tested on: Windows 11, NVIDIA T1000 4096MB (CETS Virtual PC)

<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/cbff80bb-2f2e-436b-8f2b-5421c572a762" />

<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/9284b49c-2e99-4d8b-ac84-b1b767dd4ea1" />


## Features
- CUDA path tracer with iterative bounce loop
- Camera ray generation
- Primitive inbuilt geometry (spheres and cubes)
- glTF / glB mesh loading via tinygltf (optional NODE filter, optional normalize-to-unit-box)
- Triangle intersection with Möller–Trumbore
- Mesh acceleration modes (toggleable): brute-force triangles, AABB culling, octree
- Octree built on the CPU, traversed on the GPU with nearest-first child ordering
- Materials:
  - Diffuse, Specular (mirrors + roughness)
  - Glossy (diffuse + reflective coat)
  - Emitting Cosine-weighted hemisphere sampling for diffuse BRDFs
- Stream compaction of terminated paths (thrust::partition + finalGather)
- Optional material sorting (thrust::sort_by_key) to group shading by material
- Stochastic antialiasing
- Interactive OpenGL preview with ImGui controls
  
## Quick Start

**Requirements:** CMake ≥ 3.24, CUDA Toolkit, C++17 (MSVC on Windows), OpenGL

### Build (Windows)

```powershell
mkdir build
cd build
cmake .. -G "Visual Studio 17 2022" -A x64
cmake --build . --config Release
```

The binary is written to `build/bin/Release/cis565_path_tracer.exe`.

### Run

From `build/bin/Release`:

```powershell
.\cis565_path_tracer.exe ..\..\..\scenes\cornell.json
```

Other scenes:

```powershell
.\cis565_path_tracer.exe ..\..\..\scenes\dragon_showcase.json
.\cis565_path_tracer.exe ..\..\..\scenes\skull_spheres.json
.\cis565_path_tracer.exe ..\..\..\scenes\chessboard_cubes.json
```
### Controls

- **Esc** — save image and exit
- **S** — save image (filename printed in the console)
- **Space** — re-center the camera on the original look-at

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
<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/7784f677-4042-4f8c-89f0-96fa762defbc" />


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
