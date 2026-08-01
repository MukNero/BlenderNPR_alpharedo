# Known Limitations

1. `alpha_threshold` remains quantized to u16 and included in the shader UUID as a compilation constant. Animating or driving many distinct thresholds can generate additional shader variants. Moving threshold to a runtime material constant is the preferred future optimization.
2. HASHED surface and shadow output is stochastic by design. Pixel-exact output can vary slightly by GPU, driver, and sample count.
3. The package contains all frozen CUDA, OptiX, HIP, HIPRT, and oneAPI artifacts, but every backend was not executed on representative NVIDIA, AMD, and Intel hardware. Windows EEVEE and Cycles CPU + OIDN were tested.
4. The mixed-engine migration test uses a synthetic three-scene fixture. Separate historical Blender 4.1 and 4.2 files were also tested.
5. Reproduction requires the included corrected validation suite. The original runner had Windows argument handling, AgX assertion, and dynamic-RNA lookup defects. These corrections are test-only.
