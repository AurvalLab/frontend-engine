# 来源与许可

本 Skill 的七阶段框架来自用户提供的 `FrontendEngine.docx`，其来源记录在 [pipeline.md](pipeline.md#来源说明)。该文档未注明许可，本文件中的 MIT 声明仅适用于下列第三方改编部分，不代表整个 Skill 已获得统一的 MIT 分发许可。

## Ponytail

- 来源：[DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)，本地已安装的 5.3.2 插件包。
- 已读文件：`skills/ponytail/SKILL.md`；SHA-256：`f18008471528f0adde90a560d053f85935eed5cee26c447b7177ad24472bcf32`。
- 改编范围：主文件的最小完整实现、复用顺序、影响范围、行为保护、小规模验证与已知限制说明。
- 未复制插件执行代码、Hooks、模式切换或跨会话持久行为；本 Skill 内的工程取舍不改变独立 Ponytail 配置。

```text
MIT License

Copyright (c) 2026 DietrichGebert

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## SwiftUI Pro

- 来源：[twostraws/SwiftUI-Agent-Skill](https://github.com/twostraws/SwiftUI-Agent-Skill/tree/ce32af1693b9e78a7d22ff48001c97a304d32063)，`swiftui-pro` 2.0.0。
- 固定提交：`ce32af1693b9e78a7d22ff48001c97a304d32063`；入口为 `swiftui-pro/SKILL.md`，细则为该目录下的 `references/`。
- 改编范围：按需审查与深度优化边界、通用工程质量检查、SwiftUI 分支中的 API、状态、identity、导航、无障碍、本地化和性能指导。
- 适配：不继承固定的系统版本或设备假设；不强制每类型独立文件；不自动升级 API、迁移架构或新增依赖。本文不等同于对上游所有新 API 声明的 SDK 实测。

```text
MIT License

Copyright (c) 2026 Paul Hudson.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
