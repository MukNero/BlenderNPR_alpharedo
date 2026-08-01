# Blender 5.2 NPR Alpha Fixed — Build Report

## Identity

- Final source commit: `50238a21512ab1aca56cec99a8d2b50c79296b16`
- Blender build hash: `50238a21512a`
- Source baseline commit: `82fd4b07`
- Original source identity: `f0da4307f3ec4f954326e2a1e710a8eb8ed10990`
- Windows dependency branch: `blender-v5.2-release`
- Windows dependency commit: `60d6e96b917568278d400a4024c98da0fb777338`
- Generator: Visual Studio 17 2022
- Toolchain: Visual Studio 2022 17.14.37
- CMake: 3.31.6-msvc6
- Python: 3.13
- Final build status: exit code 0

## Patch commits

1. `f2cf99eaad4ca25646d5d6d51b43f057502e7243` — Restore legacy alpha modes in Blender 5.2 NPR
2. `18ead0f41011c05690834d33f717417a77e6e908` — Fix alpha BSL runtime static branches
3. `670aa0755e596ba76365f78726dc2b4e525afe1c` — Fix alpha threshold shader cache isolation
4. `50238a21512ab1aca56cec99a8d2b50c79296b16` — Respect alpha modes in outline occlusion

The final patch contains nine intended source files. Hydrated LFS assets and dependency directories visible in the local checkout are excluded.

## Build commands

```powershell
cd D:\BlenderNPRAlpha\patched_source
.\make.bat release x64 2022b nobuild builddir D:\BlenderNPRAlpha\build\patched_release
.\make.bat release x64 2022b builddir D:\BlenderNPRAlpha\build\patched_release
```

Formal install:

```powershell
cmake.exe --install D:\BlenderNPRAlpha\build\patched_release `
  --config Release `
  --prefix D:\BlenderAlphaRelease\stage_50238a21
```

The portable release was produced from the formal Install staging directory, not by compressing the build directory.

## Frozen dependency restoration

- 53 Cycles CUDA, OptiX, HIP, and HIPRT kernel files.
- `cycles_kernel_oneapi_aot.dll`.
- `copyright.txt`.
- Open Image Denoise 2.4.1 DLL set.

The formal build supplied OIDN 2.5.0. The release package intentionally uses the baseline 2.4.1 DLL set for dependency parity. Cycles CPU + OIDN renders passed from staging and the final unpacked ZIP.

## Package audit

- Files: 6504
- Uncompressed bytes: 848,339,580
- Portable ZIP bytes: 403,517,395
- PDB/LIB/EXP: 0
- `__pycache__`/PYC: 0
- Internal build tools: 0
- Cycles GPU kernels: 53
- oneAPI AOT DLL: present
- `copyright.txt`: present
- Extracted files matched the 6504-entry SHA-256 manifest exactly

