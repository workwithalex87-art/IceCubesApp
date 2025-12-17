# IceCubesApp 7天开发计划

## 📋 快速概览

| 项目 | 内容 |
|------|------|
| **项目名称** | IceCubesApp - Mastodon 客户端 |
| **开发周期** | 7 天 |
| **目标** | 完成 MVP（最小可行产品）的完整功能 |
| **技术栈** | Swift 6.0 + SwiftUI + iOS 18.0+ |
| **工作模式** | 每天 8-10 小时专注开发 |
| **交付成果** | 可运行的 iOS 应用 + 基础测试 |

---

## 📅 7天总体规划

```
Week 1 Development Sprint
├── Day 1 (Mon)  → 项目初始化 & 架构搭建
├── Day 2 (Tue)  → 数据模型 & 网络层
├── Day 3 (Wed)  → 认证 & 账户管理
├── Day 4 (Thu)  → 时间线 & 内容显示
├── Day 5 (Fri)  → 编辑器 & 发布功能
├── Day 6 (Sat)  → 通知 & 扩展功能
└── Day 7 (Sun)  → 优化 & 测试 & 发布准备
```

---

## 🚀 第 1 天 - 项目初始化 & 架构搭建

### 目标
建立项目基础架构，创建所有必要的包和文件夹结构

### 任务列表

#### 上午（4 小时）
- [ ] **1.1 创建 Xcode 项目**
  - 创建 iOS 项目（iOS 18.0+）
  - 配置基础设置（Bundle ID、Team ID、签名）
  - 预计时间：30 分钟

- [ ] **1.2 创建 Swift Packages 结构**
  - 创建 13 个包：
    - 基础包：Models, NetworkClient, DesignSystem, Env
    - 功能包：Account, Timeline, StatusKit, Notifications
    - 其他包：Conversations, Explore, Lists, MediaUI, AppAccount
  - 预计时间：1 小时

- [ ] **1.3 配置项目设置**
  - 配置 xcconfig 文件
  - 设置 Build Settings
  - 添加 CocoaPods/SPM 依赖
  - 预计时间：1.5 小时

#### 下午（4 小时）
- [ ] **1.4 创建应用程序入口**
  - 编写 IceCubesApp.swift (@main)
  - 创建基础 ContentView
  - 设置主 TabView
  - 预计时间：1.5 小时

- [ ] **1.5 搭建 DesignSystem**
  - 定义颜色系统（Colors.swift）
  - 定义字体系统（Typography.swift）
  - 创建基础 UI 组件框架
  - 预计时间：1.5 小时

- [ ] **1.6 设置依赖注入系统**
  - 创建 Env 包的基础框架
  - 设置 Router（路由）
  - 配置环境对象注入
  - 预计时间：1 小时

### 完成标准
✅ 项目能成功构建运行（显示空白屏幕）
✅ 所有 13 个包都创建完成
✅ 基础项目结构清晰

### 代码示例

```swift
// IceCubesApp.swift
import SwiftUI

@main
struct IceCubesApp: App {
    @State private var accountManager = AppAccountsManager()
    @State private var router = Router()

    var body: some Scene {
        WindowGroup {
            if accountManager.isLoggedIn {
                ContentView()
                    .environment(accountManager)
                    .environment(router)
            } else {
                LoginView()
                    .environment(accountManager)
            }
        }
    }
}

// ContentView.swift
struct ContentView: View {
    @Environment(Router.self) private var router

    var body: some View {
        TabView(selection: $router.selectedTab) {
            TimelineView()
                .tabItem {
                    Label("Home", systemImage: "house.fill")
                }
                .tag(Tab.home)

            ExploreView()
                .tabItem {
                    Label("Explore", systemImage: "magnifyingglass")
                }
                .tag(Tab.explore)

            NotificationsView()
                .tabItem {
                    Label("Notifications", systemImage: "bell.fill")
                }
                .tag(Tab.notifications)

            AccountView()
                .tabItem {
                    Label("Account", systemImage: "person.fill")
                }
                .tag(Tab.account)
        }
    }
}
```

---

## 📱 第 2 天 - 数据模型 & 网络层

### 目标
建立完整的数据模型和网络通信基础

### 任务列表

#### 上午（4 小时）
- [ ] **2.1 创建核心数据模型**
  - Status（帖子）模型：id、content、account、mediaAttachments、createdAt
  - Account（账户）模型：id、username、displayName、avatar、note
  - Notification（通知）模型：id、type、account、status、createdAt
  - Media（媒体）模型：id、type、url、description
  - Tag（标签）模型：name、url、history
  - 预计时间：2 小时

- [ ] **2.2 实现 Codable 协议**
  - 为所有模型添加 Codable 支持
  - 处理 JSON 日期解析
  - 创建 DateFormatter 工具
  - 预计时间：1.5 小时

- [ ] **2.3 添加计算属性和方法**
  - Status：isReply、isThreadStarter、displayText
  - Account：isVerified、followerCount、followingCount
  - 预计时间：0.5 小时

#### 下午（4 小时）
- [ ] **2.4 创建 API 客户端框架**
  - 创建 Client.swift（主客户端类）
  - 定义 APIEndpoint 协议
  - 实现 URLSession 扩展
  - 预计时间：1.5 小时

- [ ] **2.5 实现 API 端点**
  - StatusEndpoints：获取时间线、发布帖子、点赞等
  - AccountEndpoints：获取账户、更新资料等
  - AuthEndpoints：登录、注册、刷新 token
  - 预计时间：1.5 小时

- [ ] **2.6 添加错误处理和日志**
  - 定义 APIError 枚举
  - 实现错误处理机制
  - 添加调试日志
  - 预计时间：1 小时

### 完成标准
✅ 所有核心模型定义完成
✅ API 客户端框架可以进行 API 调用
✅ 错误处理机制完整

### 代码示例

```swift
// Models/Status.swift
struct Status: Codable, Identifiable {
    let id: String
    let content: String
    let account: Account
    let createdAt: Date
    let mediaAttachments: [Media]
    let favouritesCount: Int
    let reblogsCount: Int
    let repliesCount: Int

    var displayText: String {
        content.replacingOccurrences(of: "<[^>]+>", with: "", 
                                     options: .regularExpression)
    }

    var isReply: Bool {
        inReplyToId != nil
    }

    enum CodingKeys: String, CodingKey {
        case id, content, account
        case createdAt = "created_at"
        case mediaAttachments = "media_attachments"
        case favouritesCount = "favourites_count"
        case reblogsCount = "reblogs_count"
        case repliesCount = "replies_count"
        case inReplyToId = "in_reply_to_id"
    }
}

// NetworkClient/Client.swift
@Observable
class Client {
    private var accessToken: String?
    private let baseURL: URL
    private let session = URLSession.shared

    init(baseURL: URL, accessToken: String? = nil) {
        self.baseURL = baseURL
        self.accessToken = accessToken
    }

    // 获取首页时间线
    func getHomeTimeline(limit: Int = 20) async throws -> [Status] {
        let endpoint = "/api/v1/timelines/home"
        let url = baseURL.appendingPathComponent(endpoint)
        var request = URLRequest(url: url)
        
        if let token = accessToken {
            request.setValue("Bearer \(token)", 
                           forHTTPHeaderField: "Authorization")
        }

        let (data, _) = try await session.data(for: request)
        return try JSONDecoder().decode([Status].self, from: data)
    }

    // 发布帖子
    func postStatus(content: String, 
                   mediaIds: [String] = [],
                   inReplyToId: String? = nil) async throws -> Status {
        let endpoint = "/api/v1/statuses"
        let url = baseURL.appendingPathComponent(endpoint)
        
        var components = URLComponents(url: url, resolvingAgainstBaseURL: false)!
        components.queryItems = [
            URLQueryItem(name: "status", value: content),
        ]
        
        if let replyId = inReplyToId {
            components.queryItems?.append(
                URLQueryItem(name: "in_reply_to_id", value: replyId)
            )
        }

        var request = URLRequest(url: components.url!)
        request.httpMethod = "POST"
        request.setValue("Bearer \(accessToken ?? "")", 
                        forHTTPHeaderField: "Authorization")

        let (data, _) = try await session.data(for: request)
        return try JSONDecoder().decode(Status.self, from: data)
    }
}
```

---

## 🔐 第 3 天 - 认证 & 账户管理

### 目标
实现用户登录和账户管理功能

### 任务列表

#### 上午（4 小时）
- [ ] **3.1 创建认证流程**
  - 创建 OAuth2 认证协议
  - 实现授权代码流程
  - 处理访问令牌存储
  - 预计时间：2 小时

- [ ] **3.2 创建登录视图**
  - LoginView（实例选择）
  - AuthorizationView（授权屏幕）
  - TokenHandlingView（处理回调）
  - 预计时间：1.5 小时

- [ ] **3.3 实现 KeyChain 存储**
  - 安全存储访问令牌
  - 安全存储刷新令牌
  - 创建 KeychainManager 工具类
  - 预计时间：0.5 小时

#### 下午（4 小时）
- [ ] **3.4 创建账户管理器**
  - AppAccountsManager（多账户管理）
  - 支持账户切换
  - 账户持久化存储
  - 预计时间：2 小时

- [ ] **3.5 创建账户视图**
  - AccountView（账户主页）
  - ProfileEditView（编辑资料）
  - SettingsView（设置）
  - 预计时间：1.5 小时

- [ ] **3.6 实现登出和错误处理**
  - 登出功能
  - Token 过期处理
  - 错误提示
  - 预计时间：0.5 小时

### 完成标准
✅ 能完整登录到 Mastodon 实例
✅ 访问令牌安全存储
✅ 可显示当前用户信息

### 代码示例

```swift
// Account/Models/AuthenticationState.swift
@Observable
class AuthenticationState {
    var isAuthenticating = false
    var error: Error?
    var instance: Instance?

    func authenticate(instanceURL: URL) async {
        isAuthenticating = true
        defer { isAuthenticating = false }
        
        do {
            // 获取实例信息
            self.instance = try await fetchInstance(url: instanceURL)
        } catch {
            self.error = error
        }
    }
}

// Account/Views/LoginView.swift
struct LoginView: View {
    @State private var instanceURL = ""
    @State private var authState = AuthenticationState()
    @State private var showingError = false

    var body: some View {
        NavigationView {
            VStack(spacing: 20) {
                Text("Choose Your Instance")
                    .font(.title2)
                    .fontWeight(.bold)

                TextField("Instance URL", text: $instanceURL)
                    .textFieldStyle(.roundedBorder)
                    .textContentType(.URL)
                    .keyboardType(.URL)

                Button(action: { Task { await login() } }) {
                    if authState.isAuthenticating {
                        ProgressView()
                    } else {
                        Text("Login")
                    }
                }
                .buttonStyle(.primaryAction)
                .disabled(instanceURL.isEmpty || authState.isAuthenticating)

                Spacer()
            }
            .padding()
            .navigationTitle("Welcome")
        }
        .alert("Error", isPresented: $showingError, 
               presenting: authState.error) { _ in
            Button("OK") { authState.error = nil }
        } message: { error in
            Text(error.localizedDescription)
        }
    }

    private func login() async {
        await authState.authenticate(
            instanceURL: URL(string: instanceURL) ?? URL(string: "https://mastodon.social")!
        )
    }
}
```

---

## 📱 第 4 天 - 时间线 & 内容显示

### 目标
实现时间线浏览和帖子显示功能

### 任务列表

#### 上午（4 小时）
- [ ] **4.1 创建时间线视图**
  - TimelineView（主时间线）
  - 支持多个时间线类型（Home、Local、Federated、Trending）
  - 时间线标题菜单
  - 预计时间：1.5 小时

- [ ] **4.2 创建帖子行组件**
  - StatusRow（帖子显示行）
  - 显示头像、用户名、时间戳
  - 显示帖子内容
  - 媒体预览
  - 预计时间：1.5 小时

- [ ] **4.3 实现无限滚动**
  - 分页加载
  - 加载更多指示器
  - 错误重试机制
  - 预计时间：1 小时

#### 下午（4 小时）
- [ ] **4.4 创建帖子详情视图**
  - StatusDetailView
  - 完整帖子内容显示
  - 回复线程显示
  - 预计时间：1.5 小时

- [ ] **4.5 实现互动功能**
  - 点赞/取消点赞
  - 转发/取消转发
  - 回复按钮
  - 菜单操作
  - 预计时间：1.5 小时

- [ ] **4.6 添加刷新和缓存**
  - Pull-to-refresh 功能
  - 基础缓存机制
  - 加载状态显示
  - 预计时间：1 小时

### 完成标准
✅ 能显示完整的时间线
✅ 支持滚动和刷新
✅ 能与帖子交互（赞、转发）

### 代码示例

```swift
// Timeline/Views/TimelineView.swift
struct TimelineView: View {
    @Environment(Client.self) private var client
    @State private var statuses: [Status] = []
    @State private var isLoading = false
    @State private var error: Error?
    @State private var timelineType: TimelineType = .home

    enum TimelineType {
        case home, local, federated, trending
        
        var endpoint: String {
            switch self {
            case .home: return "/api/v1/timelines/home"
            case .local: return "/api/v1/timelines/public?local=true"
            case .federated: return "/api/v1/timelines/public"
            case .trending: return "/api/v1/trends/statuses"
            }
        }
    }

    var body: some View {
        NavigationView {
            List(statuses) { status in
                NavigationLink(destination: StatusDetailView(status: status)) {
                    StatusRow(status: status)
                }
            }
            .navigationTitle("Home")
            .refreshable {
                await loadTimeline()
            }
            .task {
                await loadTimeline()
            }
            .overlay {
                if isLoading && statuses.isEmpty {
                    ProgressView()
                }
            }
            .alert("Error", isPresented: .constant(error != nil), 
                   presenting: error) { _ in
                Button("Retry") { Task { await loadTimeline() } }
            } message: { error in
                Text(error.localizedDescription)
            }
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

// Timeline/Views/StatusRow.swift
struct StatusRow: View {
    let status: Status

    var body: some View {
        VStack(alignment: .leading, spacing: 8) {
            // 用户信息
            HStack(spacing: 8) {
                AsyncImage(url: status.account.avatar) { image in
                    image.resizable()
                        .scaledToFill()
                } placeholder: {
                    Color.gray
                }
                .frame(width: 48, height: 48)
                .cornerRadius(8)

                VStack(alignment: .leading, spacing: 2) {
                    Text(status.account.displayName)
                        .fontWeight(.semibold)
                    Text("@\(status.account.username)")
                        .font(.caption)
                        .foregroundColor(.secondary)
                }
                Spacer()
            }

            // 内容
            Text(status.displayText)
                .lineLimit(nil)

            // 媒体
            if !status.mediaAttachments.isEmpty {
                MediaGrid(attachments: status.mediaAttachments)
            }

            // 互动统计
            HStack(spacing: 16) {
                Label("\(status.repliesCount)", 
                      systemImage: "bubble.right")
                    .font(.caption)
                    .foregroundColor(.secondary)

                Label("\(status.reblogsCount)", 
                      systemImage: "repeat")
                    .font(.caption)
                    .foregroundColor(.secondary)

                Label("\(status.favouritesCount)", 
                      systemImage: "heart")
                    .font(.caption)
                    .foregroundColor(.secondary)
            }
        }
        .padding(.vertical, 8)
    }
}
```

---

## ✍️ 第 5 天 - 编辑器 & 发布功能

### 目标
实现完整的帖子编辑和发布功能

### 任务列表

#### 上午（4 小时）
- [ ] **5.1 创建编辑器视图**
  - StatusEditorView（主编辑界面）
  - 文本编辑区域
  - 字符计数显示
  - 预计时间：1.5 小时

- [ ] **5.2 实现媒体选择**
  - ImagePicker（图片选择）
  - 支持最多 4 张图片
  - 图片预览和删除
  - 图片压缩处理
  - 预计时间：1.5 小时

- [ ] **5.3 创建编辑器状态管理**
  - StatusEditorState（编辑状态）
  - 草稿保存
  - 历史记录
  - 预计时间：1 小时

#### 下午（4 小时）
- [ ] **5.4 实现高级编辑功能**
  - 表情符号选择器
  - 投票创建
  - 内容警告
  - 可见性设置（Public、Unlisted、Private、Direct）
  - 预计时间：2 小时

- [ ] **5.5 实现帖子发布**
  - 发布单个帖子
  - 发布线程（最多 5 个）
  - 发布进度指示
  - 成功/错误处理
  - 预计时间：1 小时

- [ ] **5.6 实现回复和编辑**
  - 回复现有帖子
  - 编辑已发布帖子
  - 删除帖子
  - 预计时间：1 小时

### 完成标准
✅ 能成功发布帖子
✅ 支持媒体上传
✅ 所有高级选项可用

### 代码示例

```swift
// StatusKit/Views/StatusEditorView.swift
struct StatusEditorView: View {
    @Environment(Client.self) private var client
    @State private var editorState = StatusEditorState()
    @State private var isPosting = false
    @State private var error: Error?
    @State private var showImagePicker = false

    var body: some View {
        NavigationView {
            VStack(spacing: 16) {
                // 文本编辑区域
                TextEditor(text: $editorState.text)
                    .frame(minHeight: 100)
                    .border(Color.gray.opacity(0.3))
                    .cornerRadius(8)

                // 字符计数
                HStack {
                    Text("\(editorState.text.count)/500")
                        .font(.caption)
                        .foregroundColor(
                            editorState.text.count > 500 ? .red : .secondary
                        )
                    Spacer()
                }

                // 媒体显示
                if !editorState.selectedImages.isEmpty {
                    ScrollView(.horizontal, showsIndicators: false) {
                        HStack(spacing: 8) {
                            ForEach(editorState.selectedImages, 
                                    id: \.self) { image in
                                Image(uiImage: image)
                                    .resizable()
                                    .scaledToFill()
                                    .frame(width: 80, height: 80)
                                    .cornerRadius(8)
                            }
                        }
                    }
                }

                // 工具栏
                HStack(spacing: 12) {
                    Button(action: { showImagePicker = true }) {
                        Image(systemName: "photo")
                    }
                    .disabled(editorState.selectedImages.count >= 4)

                    Button(action: { editorState.showingEmojiPicker = true }) {
                        Image(systemName: "smiley")
                    }

                    Spacer()

                    Button(action: { Task { await postStatus() } }) {
                        if isPosting {
                            ProgressView()
                        } else {
                            Text("Post")
                        }
                    }
                    .disabled(editorState.text.isEmpty || isPosting)
                }

                Spacer()
            }
            .padding()
            .navigationTitle("New Post")
        }
        .sheet(isPresented: $showImagePicker) {
            ImagePicker(images: $editorState.selectedImages)
        }
        .alert("Error", isPresented: .constant(error != nil), 
               presenting: error) { _ in
            Button("OK") { error = nil }
        } message: { error in
            Text(error.localizedDescription)
        }
    }

    private func postStatus() async {
        isPosting = true
        defer { isPosting = false }

        do {
            _ = try await client.postStatus(
                content: editorState.text
            )
            // 成功 - 关闭编辑器
        } catch {
            self.error = error
        }
    }
}

// StatusKit/Models/StatusEditorState.swift
@Observable
class StatusEditorState {
    var text: String = ""
    var selectedImages: [UIImage] = []
    var visibility: Visibility = .public
    var showingEmojiPicker = false
    var isSensitive = false
    var contentWarning: String = ""

    enum Visibility: String {
        case public = "public"
        case unlisted = "unlisted"
        case private = "private"
        case direct = "direct"
    }
}
```

---

## 🔔 第 6 天 - 通知 & 扩展功能

### 目标
实现通知系统和其他核心功能

### 任务列表

#### 上午（4 小时）
- [ ] **6.1 创建通知视图**
  - NotificationsView（通知列表）
  - 通知行组件
  - 按类型过滤通知
  - 预计时间：1.5 小时

- [ ] **6.2 实现探索/搜索**
  - ExploreView（探索主页）
  - SearchView（搜索功能）
  - 搜索结果显示
  - 热门标签和用户
  - 预计时间：1.5 小时

- [ ] **6.3 创建直接消息**
  - ConversationsView（消息列表）
  - ChatView（聊天视图）
  - 消息发送和接收
  - 预计时间：1 小时

#### 下午（4 小时）
- [ ] **6.4 实现列表功能**
  - ListsView（列表管理）
  - ListDetailView（列表内容）
  - 创建/编辑/删除列表
  - 列表中的帖子显示
  - 预计时间：1.5 小时

- [ ] **6.5 创建用户资料页面**
  - ProfileView（用户资料）
  - 用户状态列表
  - 关注/取消关注
  - 资料编辑
  - 预计时间：1.5 小时

- [ ] **6.6 实现实时更新**
  - WebSocket 连接
  - 实时时间线更新
  - 实时通知推送
  - 预计时间：1 小时

### 完成标准
✅ 通知系统完整运行
✅ 搜索功能可用
✅ 消息功能可用
✅ 用户资料可查看

### 代码示例

```swift
// Notifications/Views/NotificationsView.swift
struct NotificationsView: View {
    @Environment(Client.self) private var client
    @State private var notifications: [Notification] = []
    @State private var isLoading = false

    enum FilterType {
        case all, mentions, favorites, reblogs, follows
    }

    @State private var selectedFilter: FilterType = .all

    var body: some View {
        NavigationView {
            List(filteredNotifications) { notification in
                NotificationRow(notification: notification)
            }
            .navigationTitle("Notifications")
            .task {
                await loadNotifications()
            }
            .refreshable {
                await loadNotifications()
            }
        }
    }

    var filteredNotifications: [Notification] {
        notifications.filter { notif in
            switch selectedFilter {
            case .all: return true
            case .mentions: return notif.type == "mention"
            case .favorites: return notif.type == "favourite"
            case .reblogs: return notif.type == "reblog"
            case .follows: return notif.type == "follow"
            }
        }
    }

    private func loadNotifications() async {
        isLoading = true
        defer { isLoading = false }

        do {
            notifications = try await client.getNotifications()
        } catch {
            // 处理错误
        }
    }
}

// Explore/Views/ExploreView.swift
struct ExploreView: View {
    @Environment(Client.self) private var client
    @State private var trendingTags: [Tag] = []
    @State private var trendingAccounts: [Account] = []
    @State private var searchText: String = ""

    var body: some View {
        NavigationView {
            ScrollView {
                VStack(alignment: .leading, spacing: 20) {
                    // 搜索栏
                    HStack {
                        Image(systemName: "magnifyingglass")
                            .foregroundColor(.gray)
                        TextField("Search", text: $searchText)
                            .textFieldStyle(.roundedBorder)
                    }
                    .padding(.horizontal)

                    // 热门标签
                    VStack(alignment: .leading, spacing: 12) {
                        Text("Trending Tags")
                            .font(.headline)
                            .padding(.horizontal)

                        ForEach(trendingTags.prefix(5), id: \.self) { tag in
                            VStack(alignment: .leading) {
                                Text("#\(tag.name)")
                                    .fontWeight(.semibold)
                                Text("\(tag.history?.first?.uses ?? 0) posts")
                                    .font(.caption)
                                    .foregroundColor(.secondary)
                            }
                            .padding()
                            .frame(maxWidth: .infinity, alignment: .leading)
                            .background(Color(.systemGray6))
                            .cornerRadius(8)
                            .padding(.horizontal)
                        }
                    }

                    // 热门账户
                    VStack(alignment: .leading, spacing: 12) {
                        Text("Suggested Accounts")
                            .font(.headline)
                            .padding(.horizontal)

                        ForEach(trendingAccounts.prefix(5), id: \.self) { account in
                            HStack(spacing: 12) {
                                AsyncImage(url: account.avatar)
                                    .frame(width: 40, height: 40)
                                    .cornerRadius(8)

                                VStack(alignment: .leading) {
                                    Text(account.displayName)
                                        .fontWeight(.semibold)
                                    Text("@\(account.username)")
                                        .font(.caption)
                                        .foregroundColor(.secondary)
                                }
                                Spacer()
                            }
                            .padding()
                            .background(Color(.systemGray6))
                            .cornerRadius(8)
                            .padding(.horizontal)
                        }
                    }
                }
            }
            .navigationTitle("Explore")
            .task {
                await loadTrendingContent()
            }
        }
    }

    private func loadTrendingContent() async {
        do {
            trendingTags = try await client.getTrendingTags()
            trendingAccounts = try await client.getTrendingAccounts()
        } catch {
            // 处理错误
        }
    }
}
```

---

## 🧪 第 7 天 - 优化、测试 & 发布准备

### 目标
进行最终优化、测试和发布前准备

### 任务列表

#### 上午（4 小时）
- [ ] **7.1 代码审查和格式化**
  - 运行 SwiftFormat
  - 检查代码风格一致性
  - 移除未使用的代码
  - 预计时间：1 小时

- [ ] **7.2 性能优化**
  - 优化列表性能（LazyVStack）
  - 图片加载优化
  - 内存泄漏检查
  - 预计时间：1.5 小时

- [ ] **7.3 错误处理和日志**
  - 添加全局错误处理
  - 网络错误提示
  - 崩溃日志
  - 预计时间：1.5 小时

#### 下午（4 小时）
- [ ] **7.4 单元测试**
  - Models 测试
  - 网络层模拟测试
  - 视图逻辑测试
  - 预计时间：1.5 小时

- [ ] **7.5 UI/UX 测试**
  - 手动功能测试
  - 不同屏幕尺寸测试（iPhone、iPad）
  - 响应速度检查
  - 预计时间：1 小时

- [ ] **7.6 发布准备**
  - 设置应用图标
  - 配置 Build Settings
  - 创建发布版本
  - 准备 App Store 文件
  - 预计时间：1.5 小时

### 完成标准
✅ 所有代码格式统一
✅ 主要功能都有单元测试
✅ 应用在真机上稳定运行
✅ 准备好提交 App Store

### 代码示例

```swift
// 单元测试示例
import XCTest
@testable import IceCubesApp

class StatusModelTests: XCTestCase {
    func testStatusDecoding() throws {
        let json = """
        {
            "id": "123",
            "content": "Hello World",
            "account": {
                "id": "456",
                "username": "test",
                "display_name": "Test User"
            },
            "created_at": "2024-12-16T10:00:00Z",
            "media_attachments": [],
            "favourites_count": 10,
            "reblogs_count": 5,
            "replies_count": 2
        }
        """
        
        let data = json.data(using: .utf8)!
        let decoder = JSONDecoder()
        let status = try decoder.decode(Status.self, from: data)
        
        XCTAssertEqual(status.id, "123")
        XCTAssertEqual(status.content, "Hello World")
        XCTAssertEqual(status.account.username, "test")
    }

    func testStatusDisplayText() {
        let status = Status(
            id: "123",
            content: "<p>Hello <strong>World</strong></p>",
            account: mockAccount,
            createdAt: Date(),
            mediaAttachments: [],
            favouritesCount: 0,
            reblogsCount: 0,
            repliesCount: 0
        )
        
        let cleanText = status.displayText
        XCTAssertFalse(cleanText.contains("<"))
        XCTAssertTrue(cleanText.contains("Hello"))
    }
}

// 网络层模拟测试
class ClientTests: XCTestCase {
    var client: Client!
    var mockSession: MockURLSession!

    override func setUp() {
        super.setUp()
        mockSession = MockURLSession()
        client = Client(
            baseURL: URL(string: "https://mastodon.social")!,
            session: mockSession
        )
    }

    func testGetHomeTimeline() async throws {
        let mockData = mockTimelineResponse
        mockSession.data = (mockData, URLResponse())

        let statuses = try await client.getHomeTimeline()
        
        XCTAssertGreaterThan(statuses.count, 0)
        XCTAssertEqual(statuses.first?.id, "123")
    }
}
```

---

## 📊 每日工时分配表

| 天数 | 任务 | 代码量估计 | 工作小时 | 完成度 |
|------|------|----------|--------|--------|
| Day 1 | 架构搭建 | 200 行 | 8 | ⭐⭐⭐ |
| Day 2 | 数据和网络 | 800 行 | 8 | ⭐⭐⭐⭐ |
| Day 3 | 认证系统 | 600 行 | 8 | ⭐⭐⭐ |
| Day 4 | 时间线显示 | 1000 行 | 8 | ⭐⭐⭐⭐ |
| Day 5 | 编辑和发布 | 1200 行 | 8 | ⭐⭐⭐⭐ |
| Day 6 | 通知和功能 | 1500 行 | 8 | ⭐⭐⭐ |
| Day 7 | 测试和优化 | 500 行 | 8 | ⭐⭐⭐⭐ |
| **总计** | **完整应用** | **5800 行** | **56 小时** | - |

---

## 🎯 关键里程碑

```
Day 1: ✓ 项目编译成功
       ↓
Day 2: ✓ 能调用 API
       ↓
Day 3: ✓ 能成功登录
       ↓
Day 4: ✓ 能显示时间线
       ↓
Day 5: ✓ 能发布帖子
       ↓
Day 6: ✓ 完整功能演示
       ↓
Day 7: ✓ 提交发布前准备
```

---

## 🛠️ 开发工具和环境

### 必需工具
- Xcode 16.0+
- Swift 6.0+
- iOS 18.0 SDK
- CocoaPods 或 SPM（依赖管理）

### 推荐扩展和插件
- Xcode SwiftUI Preview Helper
- GitHub Copilot（代码补完）
- SimulatorStatusMagic（模拟器美化）

### 数据库和缓存
- CoreData（本地存储）或 SQLite（通过 Bodega）
- UserDefaults（小数据存储）
- KeyChain（敏感数据）

---

## 💡 开发建议

### 优先级原则
1. **Day 1-3**：完成基础架构，保证每天代码可编译
2. **Day 4-5**：快速迭代核心功能，功能优先于完美
3. **Day 6**：添加辅助功能，扩展用户体验
4. **Day 7**：质量保证，性能优化

### 时间节省技巧
- ✅ 使用 Xcode 预览加速 UI 开发
- ✅ 复用现有的 UI 组件（DesignSystem）
- ✅ 使用 Mock 数据快速开发
- ✅ 并行开发不同模块
- ✅ 使用 GitHub Copilot 加速编码

### 风险控制
- ⚠️ 每天结束前提交代码（备份）
- ⚠️ 保持简单，避免过度设计
- ⚠️ 保留 1-2 小时的缓冲时间
- ⚠️ 关键功能优先完成
- ⚠️ 边界情况测试

---

## 📋 每日检查清单

### 每日 EOD (Day End) 检查
- [ ] 代码已提交到 Git
- [ ] 项目能成功编译
- [ ] 主要功能在模拟器上可运行
- [ ] 单元测试通过（如适用）
- [ ] 记录已完成的任务和遇到的问题

### 问题排查快速指南

| 问题 | 解决方案 |
|------|---------|
| 编译错误 | 检查 Swift 版本、依赖版本、导入语句 |
| 运行时崩溃 | 使用 Xcode Debugger，检查网络响应 |
| 视图不显示 | 检查 @State、@Environment、navigationLink |
| 网络请求失败 | 验证 API 端点、Token、网络连接 |
| 性能问题 | 使用 Instruments 检查内存、CPU |

---

## 🚀 快速启动脚本

### 初始化项目（Day 1）
```bash
# 创建项目目录
mkdir IceCubesApp && cd IceCubesApp

# 初始化 Git
git init

# 创建 Xcode 项目
xcodebuild -help | grep "create"

# 或者直接在 Xcode 中创建 iOS App 项目
```

### 每日构建检查
```bash
# 格式化代码
swiftformat .

# 构建项目
xcodebuild -scheme IceCubesApp build

# 运行测试
xcodebuild -scheme IceCubesApp test

# 提交更改
git add .
git commit -m "Day X: [功能描述]"
git push
```

---

## 📞 常见问题 FAQ

**Q: 7天内真的能完成吗？**  
A: 可以完成 MVP（最小可行产品）。功能可能不如完整版那么精细，但核心功能完整。

**Q: 如果某个任务超时怎么办？**  
A: 优先完成核心功能，可选功能延后。保持整体进度。

**Q: 如何处理意外问题？**  
A: 留出每日 1-2 小时的缓冲时间，关键路径出问题时可以用。

**Q: 能并行开发吗？**  
A: 可以。Day 2+ 可以多人开发不同模块（需要好的接口设计）。

**Q: 如何保证代码质量？**  
A: 代码评审、单元测试、集成测试。Day 7 专门做这些。

---

## ✅ 最终交付清单

- [ ] iOS 应用完整代码
- [ ] 所有 Swift 源文件（~5800 行）
- [ ] 单元测试代码（~500 行）
- [ ] 项目文档和注释
- [ ] 应用程序图标
- [ ] 截图和演示文档
- [ ] 发布前检查清单
- [ ] Git 提交历史记录

---

**计划制定时间**：2025 年 12 月 16 日  
**计划适用版本**：iOS 18.0+、Swift 6.0+  
**维护者**：开发团队  
**最后更新**：2025 年 12 月 16 日

---

### 祝你开发顺利! 🚀

> 记住：完成比完美更重要。先把基础功能做出来，再逐步优化和添加新功能。7 天内完成 MVP 是完全可行的！
