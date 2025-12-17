# IceCubesApp - 项目文档

## 📋 目录
1. [项目概述](#项目概述)
2. [技术栈](#技术栈)
3. [项目结构](#项目结构)
4. [核心模块](#核心模块)
5. [架构设计](#架构设计)
6. [开发环境](#开发环境)
7. [构建与测试](#构建与测试)
8. [主要功能](#主要功能)
9. [开发指南](#开发指南)

---

## 项目概述

**IceCubesApp** 是一个为去中心化社交网络 Mastodon 开发的开源客户端应用。

### 主要特点
- ✅ 完全使用 SwiftUI 构建
- ✅ 支持多平台：iOS、iPadOS、macOS、visionOS
- ✅ 轻快、高效的用户体验
- ✅ 开源项目（AGPL v3 许可证）
- ✅ 支持连接到任何 Mastodon 实例

### 发布渠道
- [App Store - Ice Cubes for Mastodon](https://apps.apple.com/us/app/ice-cubes-for-mastodon/id6444915884)

---

## 技术栈

### 核心技术
| 技术 | 版本/要求 | 说明 |
|------|----------|------|
| Swift | 6.0+ | 编程语言 |
| SwiftUI | iOS 26 SDK | UI 框架 |
| iOS 最低版本 | 18.0 | 最低部署目标 |
| visionOS | 1.0+ | Vision Pro 支持 |
| Xcode | 16.0+ | 开发工具 |

### 关键依赖库
- **Bodega**：轻量级 SQLite 数据库包装器（用于时间线缓存）
- **Swift Concurrency**：异步编程
- **SwiftFormat**：代码格式化（2 空格缩进）

### 外部 API 集成
- **Mastodon API**：社交网络功能
- **OpenAI API**：AI 辅助功能（文本修正、标签生成、图片描述）
- **DeepL API**：文本翻译
- **Apple Push Notification Service (APNS)**：推送通知

---

## 项目结构

### 项目目录树

```
IceCubesApp-main/
├── IceCubesApp/                    # 📱 主应用（~50+ 文件）
│   ├── App/                        # 应用核心入口文件
│   │   ├── IceCubesApp.swift       # @main 应用程序入口
│   │   ├── ContentView.swift       # 主内容视图（视图容器）
│   │   ├── SidebarView.swift       # 侧边栏（iPadOS/macOS）
│   │   └── MainTab.swift           # 主标签栏控制
│   ├── Assets.xcassets/            # 图片、图标、色集资源
│   │   ├── AppIcon.appiconset/     # 应用图标（所有尺寸）
│   │   ├── Colors/                 # 自定义色集
│   │   └── Images/                 # 其他图像资源
│   ├── Resources/                  # 本地化和其他资源
│   │   └── Localizable.strings     # 多语言字符串
│   ├── Info.plist                  # 应用配置
│   └── Embeds/                     # 嵌入式资源
│
├── Packages/                       # 🏗️ Swift 包（模块化架构 - 13 个包）
│   ├── Account/                    # 账户管理模块 (~30 文件)
│   │   ├── Views/
│   │   │   ├── AccountDetailView.swift
│   │   │   ├── AccountEditView.swift
│   │   │   └── PrivacySettingsView.swift
│   │   ├── Package.swift
│   │   └── Sources/
│   │
│   ├── AppAccount/                 # 应用账户管理 (~25 文件)
│   │   ├── AppAccountsManager.swift
│   │   ├── AccountViewModel.swift
│   │   └── AccountSelectionView.swift
│   │
│   ├── Conversations/              # 直接消息模块 (~35 文件)
│   │   ├── Views/
│   │   │   ├── ConversationsView.swift
│   │   │   ├── ConversationDetailView.swift
│   │   │   └── MessageComposerView.swift
│   │   ├── Models/
│   │   │   ├── Message.swift
│   │   │   └── Conversation.swift
│   │   └── Package.swift
│   │
│   ├── DesignSystem/               # 🎨 设计系统 (~100+ 文件)
│   │   ├── Sources/
│   │   │   ├── Theme.swift         # 主题系统
│   │   │   ├── Colors.swift        # 颜色定义
│   │   │   ├── Typography.swift    # 字体系统
│   │   │   ├── Components/         # 可复用 UI 组件
│   │   │   │   ├── Button.swift
│   │   │   │   ├── TextField.swift
│   │   │   │   ├── Card.swift
│   │   │   │   └── ...
│   │   │   └── Modifiers/          # SwiftUI 修饰符
│   │   └── Package.swift
│   │
│   ├── Env/                        # 🔧 环境和依赖注入 (~40 文件)
│   │   ├── Sources/
│   │   │   ├── Env.swift           # 核心环境对象
│   │   │   ├── Router.swift        # 路由管理
│   │   │   ├── CurrentAccount.swift# 当前账户
│   │   │   ├── AppPreferences.swift# 应用偏好
│   │   │   └── Services/           # 各类服务
│   │   │       ├── ThemeService.swift
│   │   │       ├── AuthService.swift
│   │   │       └── ...
│   │   └── Package.swift
│   │
│   ├── Explore/                    # 🔍 探索/搜索模块 (~45 文件)
│   │   ├── Views/
│   │   │   ├── ExploreView.swift
│   │   │   ├── SearchView.swift
│   │   │   ├── TrendingView.swift
│   │   │   └── SearchResultsView.swift
│   │   ├── Models/
│   │   │   ├── TrendingTag.swift
│   │   │   ├── SearchResult.swift
│   │   │   └── TrendingLink.swift
│   │   └── Package.swift
│   │
│   ├── Lists/                      # 📋 列表管理 (~30 文件)
│   │   ├── Views/
│   │   │   ├── ListsView.swift
│   │   │   ├── ListDetailView.swift
│   │   │   └── ListEditView.swift
│   │   ├── Models/
│   │   │   └── List.swift
│   │   └── Package.swift
│   │
│   ├── MediaUI/                    # 🖼️ 媒体查看器 (~50 文件)
│   │   ├── Views/
│   │   │   ├── ImageViewer.swift   # 图片查看和缩放
│   │   │   ├── VideoPlayer.swift   # 视频播放
│   │   │   ├── MediaGallery.swift  # 媒体画廊
│   │   │   └── MediaSharing.swift  # 分享功能
│   │   ├── Models/
│   │   │   └── MediaAttachment.swift
│   │   └── Package.swift
│   │
│   ├── Models/                     # 📦 数据模型 (~80+ 文件)
│   │   ├── Sources/
│   │   │   ├── Status.swift        # 帖子/状态模型
│   │   │   ├── Account.swift       # 账户模型
│   │   │   ├── Notification.swift  # 通知模型
│   │   │   ├── Media.swift         # 媒体模型
│   │   │   ├── Emoji.swift         # 自定义表情符号
│   │   │   ├── Tag.swift           # 标签模型
│   │   │   ├── Instance.swift      # 实例配置
│   │   │   ├── Filter.swift        # 过滤器配置
│   │   │   ├── Poll.swift          # 投票模型
│   │   │   └── ...                 # 更多模型
│   │   └── Package.swift
│   │
│   ├── NetworkClient/              # 🌐 网络通信客户端 (~100+ 文件)
│   │   ├── Sources/
│   │   │   ├── Client.swift        # 主 API 客户端
│   │   │   ├── Endpoints/          # API 端点定义
│   │   │   │   ├── StatusEndpoints.swift
│   │   │   │   ├── AccountEndpoints.swift
│   │   │   │   ├── TimelineEndpoints.swift
│   │   │   │   └── ...
│   │   │   ├── URLSession+Extensions.swift
│   │   │   ├── OpenAIClient.swift  # OpenAI 集成
│   │   │   ├── DeepLClient.swift   # DeepL 翻译
│   │   │   └── Streaming.swift     # WebSocket 流
│   │   └── Package.swift
│   │
│   ├── Notifications/              # 🔔 通知模块 (~40 文件)
│   │   ├── Views/
│   │   │   ├── NotificationsView.swift
│   │   │   ├── NotificationRow.swift
│   │   │   └── NotificationDetailView.swift
│   │   ├── Models/
│   │   │   └── Notification.swift
│   │   └── Package.swift
│   │
│   ├── StatusKit/                  # ✍️ 状态/帖子组件 (~120+ 文件)
│   │   ├── Views/
│   │   │   ├── StatusEditor.swift  # 完整编辑器
│   │   │   ├── StatusRow.swift     # 帖子行展示
│   │   │   ├── StatusDetail.swift  # 详细视图
│   │   │   ├── ImagePicker.swift   # 图片选择
│   │   │   ├── EmojiPicker.swift   # 表情符号选择
│   │   │   ├── PollEditor.swift    # 投票编辑
│   │   │   └── ...
│   │   ├── Models/
│   │   │   └── StatusEditorState.swift
│   │   └── Package.swift
│   │
│   └── Timeline/                   # 📱 时间线模块 (~150+ 文件)
│       ├── Views/
│       │   ├── TimelineView.swift  # 主时间线
│       │   ├── StatusRow.swift     # 帖子行
│       │   ├── TimelineFilterView.swift
│       │   ├── LocalTimelineView.swift
│       │   ├── FederatedView.swift
│       │   └── TrendingView.swift
│       ├── Models/
│       │   ├── Timeline.swift
│       │   ├── TimelineFilter.swift
│       │   └── TimelinePosition.swift
│       ├── Services/
│       │   ├── TimelineService.swift
│       │   ├── CacheManager.swift  # Bodega 缓存
│       │   └── SyncManager.swift   # Marker API 同步
│       └── Package.swift
│
├── IceCubesAppIntents/             # ⚡ App Intents（快捷指令）
│   ├── PostIntent.swift            # 发布帖子意图
│   ├── AppShortcuts.swift          # 快捷指令定义
│   ├── AppAccountEntity.swift      # 账户实体
│   ├── TabIntent.swift             # 标签页意图
│   ├── TimelineFilterEntity.swift   # 时间线过滤实体
│   └── ListEntity.swift            # 列表实体
│
├── IceCubesAppWidgetsExtension/    # 📲 主屏幕小部件
│   ├── IceCubesAppWidgetsExtensionBundle.swift
│   ├── AccountWidget/              # 账户小部件
│   │   └── AccountWidgetView.swift
│   ├── LatestPostsWidget/          # 最新帖子小部件
│   │   └── LatestPostsWidgetView.swift
│   ├── MentionWidget/              # 提及小部件
│   │   └── MentionWidgetView.swift
│   ├── ListsWidget/                # 列表小部件
│   │   └── ListsWidgetView.swift
│   ├── HashtagPostsWidget/         # 标签帖子小部件
│   │   └── HashtagWidgetView.swift
│   ├── Shared/                     # 小部件共享逻辑
│   │   ├── WidgetDefaults.swift
│   │   └── WidgetModels.swift
│   └── Assets.xcassets/            # 小部件资源
│
├── IceCubesNotifications/          # 🔔 推送通知服务
│   ├── NotificationService.swift   # 主服务类
│   ├── NotificationServiceSupport.swift
│   ├── IceCubesNotifications.entitlements
│   └── Info.plist
│
├── IceCubesShareExtension/         # 📤 分享扩展
│   ├── ShareViewController.swift
│   ├── IceCubesShareExtension.entitlements
│   └── Info.plist
│
├── IceCubesActionExtension/        # ⚙️ 快速操作扩展
│   ├── ActionRequestHandler.swift
│   ├── Action.js                   # JavaScript 操作脚本
│   ├── IceCubesActionExtension.entitlements
│   └── Assets.xcassets/
│
├── AlternateIcons/                 # 🎨 备选应用图标（40+ 个）
│   ├── AppIconAlternate1.icon/
│   │   ├── icon.json
│   │   └── Assets/
│   ├── AppIconAlternate2.icon/
│   ├── AppIconAlternate46.icon/
│   └── ...
│
├── Images/                         # 📸 文档和宣传图片
│   ├── promo.png                   # 宣传图
│   ├── timeline.png                # 功能截图
│   ├── editor.png
│   ├── notifications.png
│   ├── explore.png
│   ├── dm.png
│   ├── profile.png
│   └── download_on_the_app_store.svg
│
├── ci_scripts/                     # 🔄 CI/CD 脚本
│   ├── ci_pre_xcodebuild.sh        # 构建前脚本
│   └── ci_post_xcodebuild.sh       # 构建后脚本
│
├── Screenshots.pco/                # 📱 截图项目（App Store）
│   ├── project
│   └── Images/
│
├── 配置文件                         # ⚙️ 项目配置
│   ├── IceCubesApp.xcconfig.template # Xcode 构建配置模板
│   ├── IceCubesApp-release.xcconfig  # 发布配置
│   ├── .swiftformat                 # SwiftFormat 配置
│   ├── .gitignore                   # Git 忽略规则
│   └── package.resolved             # SPM 依赖锁定
│
├── 文档文件                         # 📚 项目文档
│   ├── README.md                    # 项目说明
│   ├── CLAUDE.md                    # Claude AI 指南
│   ├── AGENTS.MD                    # 代理指南
│   ├── LICENSE                      # AGPL v3 许可证
│   ├── PRIVACY.MD                   # 隐私政策
│   ├── TERMS.MD                     # 使用条款
│   └── PROJECT_DOCUMENTATION_CN.md  # 中文文档
│
└── IceCubesApp.xcodeproj/          # 📦 Xcode 项目文件
    ├── project.pbxproj             # 项目配置文件
    ├── project.xcworkspace/        # 工作空间
    └── xcshareddata/               # 共享数据
        ├── xcschemes/              # Xcode scheme 配置
        └── ...
```

### 项目统计

| 分类 | 数量 | 说明 |
|------|------|------|
| **Swift Packages** | 13 | 模块化架构包 |
| **应用扩展** | 4 | 通知、分享、操作、小部件 |
| **Swift 文件** | 500+ | 源代码文件 |
| **应用图标** | 40+ | 备选图标 |
| **支持语言** | 10+ | 国际化本地化 |

### 关键目录说明

#### 🏢 主应用层 (IceCubesApp/)
- 应用入口和全局视图容器
- 资源文件和本地化字符串
- Info.plist 配置

#### 🧩 模块层 (Packages/)
采用 **Swift Package Manager** 微服务架构，每个包独立维护：
- **功能包**：Account、Conversations、Timeline、StatusKit 等
- **基础包**：Models、NetworkClient、DesignSystem
- **支撑包**：Env、Notifications、MediaUI

#### 📲 扩展层 (Extensions/)
- NotificationService：推送通知处理
- ShareExtension：内容分享
- ActionExtension：快捷操作
- WidgetsExtension：主屏幕小部件

#### 🔧 工具层
- 构建配置：xcconfig 文件
- CI/CD 脚本：自动化工作流
- 代码格式化：SwiftFormat 配置

---

## 核心模块

### 1. **Account 模块** - 账户管理
负责用户资料显示、编辑和隐私设置管理。
- 丰富的资料支持
- 隐私设置管理
- 个人信息编辑
- 自定义字段支持

### 2. **Timeline 模块** - 时间线
核心功能：浏览和交互社交内容。
```swift
主要功能：
- 首页、本地、联邦和热门时间线
- 列表和标签时间线
- 标签组（Ice Cubes 独有功能）
- 远程本地时间线
- 引用转发功能
- 实时流事件支持
- 半自动同步（使用 Mastodon Marker API）
- 时间线缓存（使用 Bodega）
- iCloud 同步标签组、远程时间线和草稿
- 服务端过滤器支持
```

### 3. **StatusKit 模块** - 帖子编辑和显示
完整的帖子编辑和展示功能。
```swift
编辑器功能：
- 完整功能的帖子编辑器
- 支持最多 5 个帖子的线程
- 上传最多 4 张图片
- AI 辅助工具（文本修正、标签生成）
- 使用 OpenAI 生成图片描述
- 自定义表情符号支持
- 投票和内容警告
- 草稿保存和恢复
- 自动语言检测
```

### 4. **Notifications 模块** - 推送通知
完整的推送通知支持。
```swift
特性：
- 全面推送通知支持
- 自定义代理服务器（保护隐私）
- 设备端解密通知内容
- 富媒体通知（使用 INSendMessageIntent API）
- 按活动类型分组（提及、收藏、转发等）
- 应用内通知分组
- 可配置的通知类型
- 正确的账户和帖子路由
```

### 5. **Explore 模块** - 探索/搜索
发现和搜索内容。
```swift
功能：
- 热门用户、标签、帖子和链接
- 分类搜索（用户、标签、帖子）
- 趋势标签的活动图表
- 完整屏幕视图
```

### 6. **Conversations 模块** - 直接消息
私聊功能。
```swift
特性：
- 专门的私信标签
- 类似聊天的 UI
- 快速回复编辑器
- 完整编辑器支持
```

### 7. **DesignSystem 模块** - 设计系统
统一的视觉设计。
```swift
包含：
- 主题系统
- 颜色定义
- 字体设置
- 可复用的 UI 组件
- 40+ 个备选应用图标
```

### 8. **MediaUI 模块** - 媒体查看
媒体内容展示。
```swift
功能：
- 图片缩放
- 视频播放
- 内容分享
```

### 9. **Models 模块** - 数据模型
Mastodon 实体的数据结构。
```swift
包含：
- 用户模型
- 状态/帖子模型
- 通知模型
- 媒体模型
- 等等
```

### 10. **NetworkClient 模块** - 网络通信
API 客户端实现。
```swift
支持：
- Mastodon API
- OpenAI API
- DeepL API
- 异步网络请求
```

### 11. **Env 模块** - 环境和依赖注入
应用级别的状态管理。
```swift
主要组件：
- AppAccountsManager：多账户管理
- Router：路由导航
- CurrentAccount：当前账户
- Theme：主题管理
- 其他共享服务
```

---

## 架构设计

### 核心架构原则

#### 1. **模块化设计**
应用分为独立的 Swift Packages，每个包负责特定的功能域：
```
Packages/
├── UI层      → Account, Timeline, StatusKit, Notifications, Explore, Conversations
├── 数据层    → Models
├── 网络层    → NetworkClient
├── 工具层    → DesignSystem, MediaUI, Env
└── 业务层    → 各个功能包
```

#### 2. **现代 SwiftUI 架构（2025 年标准）**

**核心哲学：**
- SwiftUI 作为默认 UI 范例
- 避免 UIKit 和不必要的抽象
- 关注简洁性、清晰度和自然数据流
- **不使用 ViewModels** - 使用原生 SwiftUI 数据流模式

**状态管理策略：**
```swift
@State              // 本地、短暂的视图状态
@Binding            // 视图之间的双向数据流
@Observable         // 共享状态（推荐用于新代码）
@Environment        // 应用级别的依赖注入
```

#### 3. **异步编程**
- **Swift Concurrency**：async/await 作为异步操作的默认方式
- **生命周期感知**：使用 `.task` 修饰符
- **错误处理**：优雅的 try/catch 处理
- **避免**：除非必要，否则不使用 Combine

#### 4. **数据流方向**
```
状态向下流动 ↓
用户操作向上冒泡 ↑
```

### 代码组织规范

**按功能组织：**
```
Timeline/
├── Views/
│   ├── TimelineView.swift
│   ├── StatusRow.swift
│   └── ...
├── Models/
├── Services/
└── Package.swift
```

**文件放置原则：**
- 相关代码放在同一文件中
- 使用 extensions 组织大文件
- 遵循 Swift 命名规范

### 现代 SwiftUI 实现示例

#### 共享状态（使用 @Observable）
```swift
@Observable
class AppAccountsManager {
    var currentAccount: Account?
    var availableAccounts: [Account] = []

    func switchAccount(_ account: Account) {
        currentAccount = account
    }
}

// 在 App 文件中
struct IceCubesApp: App {
    @State private var accountManager = AppAccountsManager()

    var body: some Scene {
        WindowGroup {
            ContentView()
                .environment(accountManager)
        }
    }
}
```

#### 现代异步数据加载
```swift
struct TimelineView: View {
    @Environment(Client.self) private var client
    @State private var statuses: [Status] = []
    @State private var isLoading = false
    @State private var error: Error?

    var body: some View {
        List(statuses) { status in
            StatusRow(status: status)
        }
        .task {
            await loadTimeline()
        }
        .refreshable {
            await loadTimeline()
        }
    }

    private func loadTimeline() async {
        isLoading = true
        defer { isLoading = false }

        do {
            statuses = try await client.getHomeTimeline()
        } catch {
            self.error = error
        }
    }
}
```

### 遗留代码模式

项目中存在一些旧的 MVVM 模式代码，但**新功能不应使用 ViewModels**。

---

## 开发环境

### 系统要求
- **macOS**：最新版本
- **Xcode**：16.0 或更高版本
- **Swift**：6.0 或更高版本
- **iOS SDK**：26（2025 年 6 月）

### 最低部署版本
| 平台 | 最低版本 |
|------|--------|
| iOS | 18.0 |
| iPadOS | 18.0 |
| macOS | 最新版本 |
| visionOS | 1.0 |

### 开发工具
- **代码格式化**：SwiftFormat（配置文件：`.swiftformat`）
- **缩进**：2 空格
- **包管理**：Swift Package Manager

### 环境设置

1. **克隆项目**
```bash
git clone https://github.com/[repository]/IceCubesApp.git
cd IceCubesApp
```

2. **打开项目**
```bash
open IceCubesApp.xcodeproj
```

3. **安装依赖**
所有依赖都通过 Swift Package Manager 管理，Xcode 会自动处理。

---

## 构建与测试

### 构建命令

#### 为 iOS 模拟器构建
```bash
# iPhone Air 模拟器
xcodebuild -project IceCubesApp.xcodeproj \
    -scheme IceCubesApp \
    -sdk iphonesimulator \
    -derivedDataPath build build

# 或在 Xcode 中直接构建
```

#### 为 macOS 构建
```bash
xcodebuild -project IceCubesApp.xcodeproj \
    -scheme IceCubesApp \
    -sdk macosx build
```

### 测试命令

#### 运行所有测试
在 Xcode 的 Test navigator 中运行

#### 运行特定包的测试
```bash
# Account 包
xcodebuild -scheme AccountTests test

# Models 包
xcodebuild -scheme ModelsTests test

# Network 包
xcodebuild -scheme NetworkTests test

# Timeline 包
xcodebuild -scheme TimelineTests test

# Environment 包
xcodebuild -scheme EnvTests test
```

#### 使用 Swift Package Manager 测试单个包
```bash
cd Packages/[PackageName]
swift test
```

### 代码格式化
```bash
# 格式化代码
swiftformat .

# 检查格式
swiftformat --lint .
```

---

## 主要功能

### 📱 时间线浏览
- 首页时间线
- 本地时间线
- 联邦时间线
- 热门时间线
- 个人列表
- 关注的标签
- 标签组（Ice Cubes 独有）
- 远程本地时间线（Ice Cubes 独有）
- **实时更新**：使用 Mastodon 流事件
- **位置同步**：使用 Mastodon Marker API
- **缓存管理**：本地缓存和 iCloud 同步

### ✍️ 帖子编辑
- 完整的多功能编辑器
- 线程支持（最多 5 个帖子）
- 图片上传（最多 4 张）
- AI 辅助工具
  - 文本语法修正
  - 自动标签生成
  - AI 图片描述
- 自定义表情符号
- 投票创建
- 内容警告
- 草稿保存
- 自动语言检测

### 🔔 推送通知
- 完整的推送通知支持
- 隐私保护的代理服务器
- 设备端解密
- 富媒体通知
- 活动分组
  - 提及
  - 收藏
  - 转发
  - 关注
- 通知类型配置
- 正确的账户路由

### 🔍 探索和搜索
- 热门用户搜索
- 热门标签搜索
- 热门帖子搜索
- 热门链接搜索
- 趋势分析图表
- 多条件过滤

### 💬 直接消息
- 私聊功能
- 聊天界面
- 快速回复
- 完整编辑器

### 👤 个人资料
- 丰富的资料展示
- 隐私设置管理
- 资料编辑
- 自定义字段

### 🔐 多账户管理
- 支持多个 Mastodon 账户
- 账户切换
- 安全存储
- 应用级别账户管理

### 🌈 主题系统
- 40+ 个备选应用图标
- 可自定义的主题
- 深色/浅色模式支持
- 平台适配
  - iOS：竖屏 UI
  - iPadOS：侧边栏 UI
  - macOS：专用 UI
  - visionOS：空间 UI

### 🌍 国际化
- DeepL API 翻译支持
- 实例提供的翻译支持
- 多语言界面

### 🤖 AI 功能
- OpenAI 集成
- 文本修正
- 标签生成
- 图片描述生成

### 📲 应用扩展
- **通知服务**：推送通知处理
- **分享扩展**：从其他应用分享内容
- **快速操作扩展**：快捷操作
- **主屏幕小部件**：
  - 账户小部件
  - 最新帖子小部件
  - 提及小部件
  - 列表小部件
  - 标签帖子小部件

### 📱 快捷指令支持
- App Intents
- 发布帖子
- 自定义快捷指令

---

## 开发指南

### 编写新功能

#### DO（应该做）
```swift
// ✅ 自包含的视图
struct TimelineView: View {
    @Environment(Client.self) private var client
    @State private var statuses: [Status] = []

    var body: some View {
        List(statuses) { status in
            StatusRow(status: status)
        }
        .task {
            await loadStatuses()
        }
    }

    private func loadStatuses() async {
        // 加载数据
    }
}

// ✅ 正确使用属性包装器
@State private var count = 0
@Binding var isPresented: Bool
@Environment(Theme.self) private var theme
@Observable class MyService {}

// ✅ 使用异步等待
async func loadData() {
    do {
        let data = try await fetchData()
    } catch {
        handleError(error)
    }
}

// ✅ 显式处理加载和错误状态
if isLoading {
    ProgressView()
} else if let error = error {
    ErrorView(error: error)
} else {
    ContentView(data: data)
}

// ✅ 简单的异步操作使用 Combine 的替代方案
.task {
    await performAsyncWork()
}
```

#### DON'T（不应该做）
```swift
// ❌ 为每个视图创建 ViewModel
class TimelineViewModel: ObservableObject {
    @Published var statuses: [Status] = []
}

// ❌ 不必要的状态移出
@State private var dataManager = DataManager()  // 错误！

// ❌ 过度抽象
func complexAbstraction() {
    // 不必要的间接层
}

// ❌ 嵌套 @Observable 对象
@Observable
class Service1 {
    var service2 = Service2()  // ❌ 破坏 SwiftUI 的观察系统
}
// 应该在视图级别初始化

// ❌ 简单操作使用 Combine
NotificationCenter.default
    .publisher(for: UIApplication.didBecomeActiveNotification)
    .sink { _ in
        // 这里应该用 .task
    }
```

### 代码风格

#### 命名约定
```swift
// 视图
struct TimelineView: View {}
struct StatusRow: View {}

// 服务
class AuthenticationService {}
@Observable
class UserService {}

// 数据模型
struct Status: Codable {}
struct Account: Codable {}

// 函数
func loadTimeline() async {}
func handleStatusTapped(_ status: Status) {}

// 变量
private var isLoading = false
private let apiClient = APIClient()
@State private var selectedTab: Tab = .home
```

#### 代码组织
```swift
struct TimelineView: View {
    // 属性
    @Environment(\.dismiss) var dismiss
    @State private var statuses: [Status] = []

    // 计算属性
    var isEmpty: Bool {
        statuses.isEmpty
    }

    // Body
    var body: some View {
        List(statuses) { status in
            StatusRow(status: status)
        }
    }

    // 私有方法
    private func loadStatuses() async {
        // ...
    }
}

// 扩展用于组织
extension TimelineView {
    @ViewBuilder
    var emptyState: some View {
        VStack {
            Text("No statuses")
        }
    }
}
```

### 建立验证流程（重要）

修改代码后**必须**：

1. **构建项目**
```bash
xcodebuild -project IceCubesApp.xcodeproj -scheme IceCubesApp -sdk iphonesimulator build
```

2. **修复编译错误**
确保没有编译错误才能继续

3. **运行相关测试**
```bash
xcodebuild -scheme AccountTests test
xcodebuild -scheme TimelineTests test
```

4. **验证代码风格**
运行 SwiftFormat 检查

### Git 工作流

```bash
# 创建特性分支
git checkout -b feature/new-feature

# 进行更改
# ... 编辑文件 ...

# 验证更改
git diff

# 提交更改
git add .
git commit -m "feat: add new feature description"

# 推送到远程
git push origin feature/new-feature
```

### 文档化代码

```swift
/// 加载用户时间线
/// - Parameters:
///   - accountId: 用户 ID
///   - limit: 返回的状态数量，默认为 20
/// - Returns: 状态列表
/// - Throws: 网络错误或解析错误
func loadUserTimeline(accountId: String, limit: Int = 20) async throws -> [Status] {
    // 实现
}

// 使用 MARK 组织代码
// MARK: - Lifecycle
override func viewDidLoad() {}

// MARK: - Private Methods
private func setupUI() {}

// MARK: - Helpers
private func formatDate(_ date: Date) -> String {}
```

---

## iOS 26 SDK 集成（2025 年）

### 支持的新 API

#### 液态玻璃效果
```swift
Button("Post", action: postStatus)
    .buttonStyle(.glass)
    .glassEffect(.thin, in: .rect(cornerRadius: 12))
```

#### 增强的滚动
```swift
List {
    // 内容
}
.scrollEdgeEffectStyle(.automatic, for: .vertical)
```

#### 选项卡栏增强
```swift
TabView {
    // 选项卡
}
.tabBarMinimizeBehavior(.automatic)
```

#### 拖放改进
```swift
.draggable(item.id)
.dropDestination(for: String.self) { items in
    // 处理放置
}
```

#### 文本编辑增强
```swift
TextEditor(text: $text)
    // iOS 26 支持 AttributedString
```

#### 无障碍访问
```swift
#if os(iOS)
if #available(iOS 26, *) {
    // 使用 AssistiveAccess
}
#endif
```

### 使用指南
- 使用 `#available(iOS 26, *)` 条件编译 iOS 26 特性
- 在支持的地方用 iOS 26 API 替换旧实现
- 在时间线和编辑器中充分利用液态玻璃效果

---

## 故障排除

### 常见问题

#### Q: 构建失败，显示 Swift 版本错误
A: 确保 Xcode 16.0+ 和 Swift 6.0+ 已安装

#### Q: 模拟器未找到
A: 在 Xcode 中创建模拟器：Xcode → Settings → Devices and Simulators

#### Q: 测试失败
A: 运行 `swift test -v` 查看详细错误信息

#### Q: 推送通知不工作
A: 检查项目的 Apple Developer 账户设置和证书

---

## 资源和参考

### 官方文档
- [SwiftUI 官方文档](https://developer.apple.com/xcode/swiftui/)
- [Swift 并发文档](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency)
- [Mastodon API 文档](https://docs.joinmastodon.org/)

### 项目资源
- GitHub 仓库：[IceCubesApp](https://github.com/dimillian/IceCubesApp)
- 许可证：AGPL v3
- 维护者：Thomas Ricouard

### 相关库
- [Bodega](https://github.com/mergesort/Bodega) - SQLite 数据库包装器

---

## 许可证

本项目采用 **AGPL v3** 许可证。详见 [LICENSE](LICENSE) 文件。

---

## 贡献指南

欢迎贡献！请遵循以下步骤：

1. Fork 项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

---

**文档更新时间**：2025 年 12 月 16 日  
**文档版本**：1.0  
**项目名称**：IceCubesApp - Mastodon 客户端  
**开发语言**：Swift / SwiftUI
