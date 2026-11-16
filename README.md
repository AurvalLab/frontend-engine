# Frontend Engine · 前端创作

面向 Codex 的前端创作技能。按任务规模衔接设计、实现和验证，支持 Web，以及明确指定的 SwiftUI 原生界面任务。

## 工作方式

| 任务 | 流程 |
| --- | --- |
| 局部调整 | 查看现有界面与受影响组件 → 修改 → 针对性验证 |
| 单页面或新组件 | 简要设计 → 实现与视觉打磨 → 验证 |
| 整站、复杂流程或大范围重设计 | 按依赖展开设计、Tokens、实现、打磨与 QA；复杂动效另做规格与接入 |

- 需要探索视觉方向时，优先通过可用的 Product Design 生成三张独立方案，选定后再实现。
- 已有明确视觉目标时沿用；局部修改不自动重新选图。用户可明确要求跳过方案选择。
- 小任务合并步骤和记录；复杂动效才记录结构冻结。验证范围由实际影响和支持平台决定。
- 沿用现有框架、组件、设计系统和依赖，保留安全、无障碍、错误处理与动效失败回退。

保留 Stage 1–7 调用：Blueprint → Tokens → Motion Spec → Skeleton → Taste Polish → Motion → QA。可以只做指定阶段；无动效任务跳过相关阶段，不创建空文件。

## 安装与使用

在 Codex 的技能目录中克隆本仓库。使用默认目录时：

```bash
git clone https://github.com/AurvalLab/frontend-engine.git ~/.codex/skills/frontend-engine
```

Windows PowerShell：

```powershell
git clone https://github.com/AurvalLab/frontend-engine.git "$env:USERPROFILE\.codex\skills\frontend-engine"
```

使用自定义 `CODEX_HOME` 时，将安装位置替换为其 `skills/frontend-engine` 子目录。已有非 Git 管理的同名技能时，先备份，再同步本仓库的技能文件。

调用示例：

```text
使用 $frontend-engine，制作一个产品介绍页，先给我三个视觉方案。
使用 $frontend-engine，按照这张截图实现页面。
使用 $frontend-engine，只调整现有页面的移动端布局。
使用 $frontend-engine，只做 Stage 1 蓝图，先不要写代码。
```

从本仓库克隆安装后，可在技能目录中运行 `git pull --ff-only` 更新。

## 文件与可选能力

- [SKILL.md](SKILL.md)：任务分流、设计入口、实施约束与交付规则。
- [Web 阶段说明](references/pipeline.md)：七阶段职责和按风险选择的验证方式。
- [工程质量检查](references/quality.md)：API、状态、性能、无障碍与反馈。
- [SwiftUI 分支](references/swiftui.md)：原生实现与验证边界。
- [agents/openai.yaml](agents/openai.yaml)：技能显示名称与调用提示。

Product Design、Image Gen、浏览器和 Xcode 等能力取决于运行环境，本仓库不安装这些工具。未提供 Product Design 时，可先做简要文字设计；明确要求图片方案时需先解决生成能力缺口。SwiftUI 实现按原生分支执行，不因使用设计参考而改成 Web。

## 来源与许可

七阶段框架源于用户提供的 `FrontendEngine.docx`；部分工程指导改编自 Ponytail 与 SwiftUI Pro。详见[来源记录](references/pipeline.md#来源说明)和[第三方说明](references/third-party-notices.md)。

源文档未注明许可；第三方 MIT 声明仅适用于对应改编部分，本仓库不声明统一的 MIT 许可。

结构校验不等同于实际网页生成、浏览器验收、SwiftUI 编译或真机验证。实际交付需报告已检查的范围与未验证项。
