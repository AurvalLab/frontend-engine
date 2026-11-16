# SwiftUI 原生分支

仅在用户明确要求 Apple 原生界面或现有项目使用 SwiftUI 时读取。沿用主文件的任务分流、设计入口和按交接价值保存记录的规则；保留七阶段职责，以原生文件与资源履行契约，不执行 Web 的 DOM、CSS 变量、JS 失效或 CSS 像素视口检查。共享取舍在 SKILL.md，跨平台检查见 [quality.md](quality.md)。本分支改编自 SwiftUI Pro，许可见 [第三方说明](third-party-notices.md)。

## 先确认工程边界

- 读取实际工程、平台、deployment target、Swift/Xcode 配置、组件、资源和状态架构。使用可用工具读取 SDK 声明或 Apple 文档，确认具体 API 的编译与运行条件。
- 保持已有 target 和工具链配置。新运行时 API 需要兼容目标或 `#available` 分支；新编译器或 SDK 声明不能靠运行时判断让旧工具链识别。
- 不沿用上游固定的 iOS、Xcode、Swift 默认版本，也不把其设备名称和发布日期当成已核实环境事实。新项目优先采用用户已明确的目标；缺失且影响 API 选择时先确定支持范围。
- 沿用项目既有 SwiftUI、UIKit/AppKit 互操作与文件组织。只有独立职责或观察更新边界确实需要时才拆 View 或文件，不执行“每类型必须独立文件”的机械重排。
- 优先平台控件和现有组件，不为原生分支安装 UI 框架、源 Skill 或插件。需要 Xcode 的编译、预览和模拟器验证仅在实际可用环境进行。

## 七阶段映射

| 阶段 | 原生交付物与检查 |
| --- | --- |
| 1 Blueprint | 屏幕、导航、主要任务、内容状态与选定视觉目标；复杂任务保存于已有文档或 `.agent-context/blueprint.md`。以平台交互习惯规划，不输出实现代码。 |
| 2 Tokens | 沿用 Asset Catalog、语义字体、主题或共享常量；新项目可用 `DesignTokens.swift` 表达必要设计值。支持系统外观和 Dynamic Type，不创建 `tokens.css`。 |
| 3 Motion Spec | 复杂动效记录状态变化、触发、View identity、transition、取消与 Reduce Motion 行为。优先写入蓝图，内容较多时另存 `.agent-context/motion-spec.md`；基础反馈随实现完成。 |
| 4 Skeleton | 实际 SwiftUI View、导航和最小状态/数据接入。先让核心任务可用，再检查窄窗口、较大文字及必要错误状态；不输出 `index.html`。 |
| 5 Taste Polish | 在真实 Preview、模拟器或设备中比对目标，检查排版、系统控件、触控目标、空状态和外观。复杂动效才在 `[Freeze]` 或已有规格中记录结构、identity、状态归属与布局依赖。 |
| 6 Native Motion | 在已约定状态与 View 上接入动画，保持结构和数据行为。支持 Reduce Motion、任务取消和再次进入；静态需求标为不适用。不要为动效重建状态拥有者或更换 identity。 |
| 7 QA | 按影响范围验证实际支持的设备与可变窗口、文字大小和外观；运行相关构建和测试，检查导航、取消、稳定 identity 与辅助技术。轻量任务在回复中记录；复杂任务更新已有验收文档或 `.agent-context/audit-report.md`，注明真实环境及未验证项。 |

阶段内仅修改职责所需文件。简单页面可合并设计与 Tokens、实现与打磨，基础动画随实现完成，无动效不创建规格或冻结记录。保护 View identity 与状态生命周期不因简化流程而省略。结构缺陷直接修复，更新受影响约定并复测；有冻结记录时同步更新。单阶段和只设计的请求仍按主文件限制执行。

## 实现检查

### 状态、绑定与列表

- 项目目标支持时，新共享模型可用 `@Observable`；按实际 actor isolation 保护 UI 可变状态，不能盲目重复添加 `@MainActor`。旧项目中的 `ObservableObject` 等体系无需在无关工作中整体迁移。
- View 拥有的局部 `@State` 通常应为 `private`，每个状态只初始化一次。改已有可见性前检查初始化调用方，避免破坏对外参数。
- 优先属性投影的绑定。自定义 `Binding(get:set:)` 只有在行为等价时才替换：setter 可处理相同值写入，`onChange` 还会响应程序引发的真实值变化，两者不等价。
- `ForEach` 的可变数据以元素稳定 ID 标识；保持条件分支之上的选择与导航状态。不要在 `body` 或行构造回调里反复创建需要保持的引用模型。

### 导航、控件与布局

- 目标支持时采用 `NavigationStack` / `NavigationSplitView`，按数据注册目的地；同一栈中避免冲突注册和混杂两套目的地机制。可选数据驱动的呈现优先让该数据同时表达展示状态，减少不同步的 Boolean。
- 动作使用 `Button`、`Menu`、`Label` 等语义控件，提供文字名称；视觉图标模式不应丢失辅助功能名称。自定义手势保留它真正需要的交互能力。
- 按窗口可用空间适配，必要时使用 `ViewThatFits`、滚动和系统容器，不用固定“主屏幕”尺寸推断布局。安全区优先交给系统处理，不猜测边距。
- 字体使用 Dynamic Type；需要自定义尺度时采用目标支持的缩放能力。iOS 触控目标检查至少 44×44 pt，其他平台按其原生规范处理。
- 以 `accessibilityLabel` 等提供有意义的说明；装饰图片可排除。检查 VoiceOver、非颜色线索和 `accessibilityReduceMotion`，不因窗口变窄丢掉功能入口。

### 生命周期、性能与本地化

- 异步工作优先使用项目支持的 `.task` 和结构化并发；保留取消与错误处理，不把昂贵工作塞进 View 初始化器或频繁求值的 `body`。
- 保持 View 的结构 identity，修复真实的多余更新。缓存必须有完整失效路径；不要仅因读取多个 observable 属性就拆模型，或为推测性性能问题改成新的架构。
- 使用与状态值关联的动画，连续动作通过目标支持的完成机制衔接。布局改变不能破坏已确定的数据生命周期；减弱动态效果时改用温和反馈或直接呈现。
- 文案沿用现有 String Catalog、`.strings` 或 `.stringsdict`；不擅自迁移。使用本地化插值、所属 bundle 和环境中的 locale、calendar、timeZone，按 target 核对具体 API。

## 验证与交付

检查范围来自实际支持平台与改动。iPhone、iPad 或 macOS 的窗口、旋转、键盘和尺寸类别按项目需要覆盖；加入长译文、较大 Dynamic Type、浅深色与相关辅助功能场景。涉及真实硬件的行为单列真机验证，不以模拟器替代。

可用时运行项目已有的 Xcode 工具、构建、Preview、测试和模拟器。没有 macOS/Xcode 时，只交付可做的源码与静态检查，把编译、预览、VoiceOver、模拟器和真机结果列为未验证；不能声称已运行或可上架。
