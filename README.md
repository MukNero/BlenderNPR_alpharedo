# Blender 5.2 NPR Alpha Compatibility Build (Win64)

This release restores legacy EEVEE material alpha controls in the Blender 5.2 NPR port and carries them through material RNA, file versioning, EEVEE surface/shadow pipelines, shader-cache identity, and outline occlusion.

## Build identity

- Source baseline: `f0da4307f3ec4f954326e2a1e710a8eb8ed10990`
- Baseline snapshot commit: `82fd4b07`
- Final source commit: `50238a21512ab1aca56cec99a8d2b50c79296b16`
- Blender build hash: `50238a21512a`
- Windows dependencies: `blender-v5.2-release` at `60d6e96b917568278d400a4024c98da0fb777338`
- Toolchain: Visual Studio 2022 17.14.37, CMake 3.31.6-msvc6, Python 3.13

## Included fixes

- Surface Alpha Mode: OPAQUE, CLIP, HASHED, BLEND.
- Shadow Alpha Mode: NONE, OPAQUE, CLIP, HASHED.
- Legacy alpha state survives open, save, reopen, animation, and drivers.
- Surface and shadow modes enter the EEVEE compilation constants.
- Surface mode, shadow mode, and quantized threshold enter the shader UUID.
- 64-bit material UUID is folded into the 32-bit Murmur2A seed to prevent unrelated alpha variants from sharing one `GPUPass`.
- CLIP threshold equality is stable after u16 quantization.
- Outline Occlusion follows the selected alpha mode instead of treating every non-zero alpha texel as an unconditional occluder.
- Clean portable packaging restores all frozen Cycles GPU kernels, oneAPI AOT, copyright data, and OIDN 2.4.1 while excluding build artifacts.

## Changed source files

1. `scripts/startup/bl_ui/properties_material.py`
2. `source/blender/blenloader/intern/versioning_420.cc`
3. `source/blender/draw/engines/eevee/eevee_material.cc`
4. `source/blender/draw/engines/eevee/eevee_shader.cc`
5. `source/blender/draw/engines/eevee/shaders/eevee_pipeline.bsl.hh`
6. `source/blender/draw/engines/eevee/shaders/eevee_surf_depth.bsl.hh`
7. `source/blender/draw/engines/eevee/shaders/eevee_surf_shadow.bsl.hh`
8. `source/blender/gpu/intern/gpu_codegen.cc`
9. `source/blender/makesrna/intern/rna_material.cc`

Final delta: 206 insertions, 105 deletions.

## Validation

- Core Windows suite: PASS.
- Stress suite: PASS.
- 16 Surface/Shadow combinations and four thresholds persisted after save/reopen.
- Surface and Shadow real-render tests passed.
- Shader mode/threshold cache isolation passed.
- RNA fuzz: 1601 checks, zero errors.
- 80 material variants compiled and rendered.
- Outline Occlusion RGBA regression passed.
- Historical Blender 4.1/4.2 files and mixed-engine fixtures passed alpha-state migration checks.
- Cycles CPU + OpenImageDenoise 2.4.1 passed from staging and the unpacked release ZIP.
- Portable archive audit and 6504-file SHA-256 manifest comparison passed.

See `BUILD_REPORT.md`, `TEST_REPORT.md`, and `KNOWN_LIMITATIONS.md` for details.

## Package contents

- `blender-5.2.0-npr-port-win64-alpha-fixed-clean-50238a21.zip`: clean Win64 portable build.
- `blender-5.2-npr-alpha-compatibility-50238a21.patch`: final nine-file source patch.
- `alpha_debug_suite_v3_fixed.zip`: corrected reproducible Windows validation suite.
- `BUILD_REPORT.md`: reproducible build and packaging information.
- `TEST_REPORT.md`: runtime, migration, stress, outline, and archive results.
- `KNOWN_LIMITATIONS.md`: remaining risks and test coverage boundaries.
- `SHA256SUMS.txt`: hashes for release assets.

## Applying the patch

From the matching NPR source baseline:

```powershell
git apply --check blender-5.2-npr-alpha-compatibility-50238a21.patch
git apply blender-5.2-npr-alpha-compatibility-50238a21.patch
```

Release build:

```powershell
make.bat release x64 2022b builddir <build-directory>
```

## Known limitations

- `alpha_threshold` remains a quantized u16 compilation constant. Animating many unique threshold values can generate additional shader variants. A future optimization should move the threshold to a runtime material constant while keeping only the modes compile-time.
- HASHED surface and shadow output is stochastic and may differ slightly across hardware, drivers, and sample counts.
- All frozen GPU backend files are included, but every CUDA/OptiX/HIP/HIPRT/oneAPI combination was not executed on representative hardware.
- Use the included corrected validation suite. The original test runner had Windows argument handling and assertion defects; these test-only fixes do not alter product behavior.

## GitHub upload note

The portable ZIP is approximately 403 MB and exceeds GitHub's normal 100 MB repository object limit. Upload this pack or the portable ZIP as a **GitHub Release asset**, or track it with Git LFS. Do not commit it as an ordinary Git blob.
