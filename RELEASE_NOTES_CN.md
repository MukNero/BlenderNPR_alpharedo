# Blender 5.2 NPR Alpha 兼容版说明

此版本恢复了旧版 EEVEE 材质的 Surface/Shadow Alpha 模式，并使模式与阈值真正进入 5.2 EEVEE 的深度、阴影、Shader Cache 和 Outline Occlusion 路径。

## 支持状态

- Surface：OPAQUE / CLIP / HASHED / BLEND
- Shadow：NONE / OPAQUE / CLIP / HASHED
- 支持保存重开、动画和驱动
- 修复不同 Alpha Mode / Threshold 错误共享 Shader 的问题
- 修复透明区域产生错误轮廓遮挡的问题

## 验证结果

- Core 与 Stress 测试通过
- 16 种 Surface/Shadow 组合及四个阈值保存重开通过
- 80 个 Shader 变体编译与渲染通过
- RNA fuzz 1601 项、0 错误
- Surface、Shadow、Outline Occlusion 实渲通过
- Cycles CPU + OIDN 2.4.1 通过
- 最终便携包 6504 个文件逐项 SHA-256 验证通过

## 已知限制

- Threshold 仍是 u16 编译常量，大量动画阈值可能生成额外 Shader 变体。
- HASHED 模式具有随机性，不同硬件和驱动可能存在少量像素差异。
- 便携包包含完整 GPU 后端文件，但没有在所有 NVIDIA/AMD/Intel 硬件上逐项实测。

便携 ZIP 超过 GitHub 普通仓库 100 MB 限制，应作为 GitHub Release Asset 上传，或使用 Git LFS。
