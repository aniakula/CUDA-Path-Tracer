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

### Path Tracing Loop

Each sample for a pixel is a **path**: a ray that leaves the camera and bounces through the scene until it hits a light, misses everything, or runs out of bounces.

Per iteration, the GPU:

1. **Generates** one camera ray per pixel (`generateRayFromCamera`)
2. **Intersects** that ray against the scene (`computeIntersections`)
3. **Shades** the hit and updates path. Afterwards either terminates or scatters a new ray (`shadeMaterial` / `scatterRay`)
4. **Compacts** away finished paths and repeats until no paths remain or `DEPTH` is reached
5. **Accumulates** the path’s contribution into the image buffer

Throughput starts at `(1,1,1)` and is multiplied by the BRDF at each bounce, so later bounces contribute less light.

<!-- insert bouncing / path loop diagram here -->

### Camera Ray Generation

For pixel `(x, y)`, a ray originates at the camera position and aims through that pixel. The direction is built from the camera basis (`view`, `right`, `up`) and the pixel’s offset from the image center, scaled by `pixelLength`.

With antialiasing enabled, `(j_x, j_y)` are uniform random offsets in `([0,1))`, so each iteration samples a slightly different point inside the pixel.

Each path also stores `remainingBounces = DEPTH` and a `pixelIndex` so its final color can be written back to the correct pixel.

<!-- insert ray generation diagram here -->

### Intersections

`computeIntersections` walks every geometry in the scene and keeps the closest hit (`t_min`):

- **Spheres / cubes** — intersection tests with inverse transforms
- **Meshes** — each candidate triangle is tested with **Möller–Trumbore** 

### Möller–Trumbore Triangle Test

Moller–Trumbore finds the ray to triangle intersection by solving for barycentric coordinates and distance without pre computing and storing extra data for the plane equation of the triangle.

<!-- insert Möller–Trumbore diagram here -->

**Why this over other mesh tests?**
Moller–Trumbore:
- Avoids a separate plane equation and projection step
- Uses only vector ops that map well to GPUs
- Rejects many misses early via barycentric bounds
- Needs only the three vertices (no precomputed plane or edge equations)

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
  
<div align="center">

<table>
<tr>
<td align="center">
<img width="400" height="400" src="https://github.com/user-attachments/assets/894f3d0f-8c74-47d4-ace4-568d6b747c40" alt="AA">
<br>
<img width="194" height="628" alt="image" src="https://github.com/user-attachments/assets/be472e69-37bf-48b3-b26d-462cb48d430c" />
<em>Figure 1: AA visualization</em>
</td>

<td align="center">
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/d754900e-9e3b-4d2c-8c94-364933a1d9b2" />
<br>
<img width="202" height="534" alt="image" src="https://github.com/user-attachments/assets/46104194-dc17-450b-bad2-bf42850ad075" />

<em>Figure 2: No AA visualization</em>
</td>
</tr>
</table>

</div>

- show different materials rendered

<div align="center">
<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/7784f677-4042-4f8c-89f0-96fa762defbc" />
</div>

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
