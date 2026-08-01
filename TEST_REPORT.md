# Blender 5.2 NPR Alpha Fixed — Test Report

## Release status

- Alpha source: PASS
- Windows runtime: PASS
- Clean portable package: PASS on the tested Windows CPU/GPU-driver environment

All final release checks were repeated from the executable extracted from the final portable ZIP.

## Core suite

- Process return code: 0
- `FINAL_PASS.txt`: present
- Every log contains `STRICT_PASS:`
- Every top-level JSON `errors` array: empty
- Surface mapping: OPAQUE, CLIP, HASHED, BLEND
- Shadow mapping: NONE, OPAQUE, CLIP, HASHED
- Save/reopen persistence passed for all 16 Surface/Shadow combinations and four thresholds
- Surface real render: PASS
- Shadow real render: PASS
- Shader cache mode and threshold isolation: PASS
- RNA fuzz: 1601 checks, 0 errors

## Stress suite

- Process return code: 0
- `FINAL_PASS.txt`: present
- 80 Surface/Shadow/Threshold material variants compiled and rendered
- Shader stress PNG and Blend evidence: present
- RNA fuzz: 1601 checks, 0 errors

## Outline Occlusion regression

A real RGBA texture was used to test OPAQUE, CLIP, HASHED, and BLEND. Transparent texels no longer behave as unconditional outline occluders, while OPAQUE behavior remains stable. Before/after PNG evidence is included in the full validation archive retained with the build records.

## Threshold variant measurements

- First independent 80-variant run: 2.395 s
- First-run peak working set: 0.788 GiB
- First-run peak private bytes: 2.073 GiB
- Second reopen: 2.183 s
- Second-run peak working set: 0.776 GiB
- Second-run peak private bytes: 2.486 GiB
- Dumped shader-stage files: 880
- STRESS materials represented: 80/80
- Threshold animation mean render time:
  - first 19 unique thresholds: 0.1042 s
  - repeated 19 thresholds: 0.0740 s
  - 18 additional unique thresholds: 0.0811 s

## File migration coverage

Alpha state was checked through copy, save, reopen, and comparison using historical Blender 4.1/4.2 files, legacy NPR fixtures, and a three-scene EEVEE/CYCLES/WORKBENCH mixed-engine fixture. Original source files remained unchanged and verifier JSON files contained no errors.

## Cycles and OIDN

- Engine: CYCLES
- Device: CPU
- Samples: 4
- Denoiser: OPENIMAGEDENOISE
- OIDN package version: 2.4.1
- Staging render: PASS
- Final unpacked-ZIP render: PASS

## Final archive verification

- ZIP central-directory audit: PASS
- Forbidden artifacts: 0
- Manifest comparison after extraction: 6504/6504 exact matches
- Core from unpacked ZIP: PASS
- Stress from unpacked ZIP: PASS
- OIDN from unpacked ZIP: PASS
- Outline Occlusion from unpacked ZIP: PASS

