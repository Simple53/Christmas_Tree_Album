# 🎄 Merry Christmas 3D Interactive Web App

这是一个基于 **Three.js** 和 **MediaPipe** 的互动 3D 圣诞树网页应用。它不仅展示了绚丽的 3D 粒子效果，还支持**手势控制**、**照片云存储**（GitHub）以及**背景音乐**功能。通过手势或鼠标交互，你可以探索这个充满节日氛围的虚拟世界。

---

## ✨ 核心功能 (Features)

1.  **3D 粒子圣诞树**: 使用数百个动态粒子组成的旋转圣诞树，支持两种形态切换（树形 vs 散射圆环）。
2.  **AI 手势控制**: 集成 Google MediaPipe 手势识别，通过摄像头实现无接触交互：
    *   👊 **握拳 (Fist)**: 召唤圣诞树形态。
    *   🖐 **张开手掌 (Open Palm)**: 切换到散射（Scatter）模式，粒子组成巨大的光环。
    *   ☝️ **食指指点 (Pointing)**: 聚焦/放大当前选中的照片（Zoom 模式）。
    *   ↔️ **左右倾斜手掌**: 在查看照片时切换上一张/下一张。
3.  **照片管理系统**:
    *   **上传**: 支持本地上传照片，自动通过 GitHub API 上传到云端仓库（需配置），实现永久保存。
    *   **管理**: 提供可视化的照片管理面板，支持查看、清空所有或单独删除照片。
    *   **同步**: 支持混合存储模式（Local Storage + GitHub），自动同步云端照片。
4.  **节日氛围**:
    *   **背景音乐**: 自动播放经典的《Jingle Bells》，提供静音开关。
    *   **雪花特效**: 飘落的 3D 雪花。
    *   **倒计时/标题**: 巨大的 "MERRY CHRISTMAS" 动态标题。
5.  **跨平台适配**: 针对移动端进行了 UI 优化，支持手机浏览（建议横屏或在性能较好的设备上体验）。

---

## 🛠️ 代码结构 (Code Structure)

本项目采用**单文件架构 (`index.html`)**，方便部署和分享。核心逻辑位于 `<script type="module">` 块中。

### 主要类说明：

*   **`App`**: 应用程序的主入口。负责初始化场景、输入管理器、UI 事件绑定（点击、拖拽、弹窗管理）以及主循环 (`animate`)。
*   **`World`**: 3D 世界的核心容器。管理场景 (`Scene`)、相机 (`Camera`)、渲染器 (`Renderer`) 以及所有的 3D 实体（粒子、照片、雪花等）。
*   **`InputManager`**: 处理交互输入。不仅监听鼠标/触摸事件，还封装了 `MediaPipe HandLandmarker`，负责从摄像头视频流中实时解析手势逻辑。
*   **`PhotoManager`**: **核心功能模块**。负责照片的加载、布局（在树上的螺旋排列 vs 散射排列）、上传、删除以及与 GitHub API 的交互。
*   **`GithubStore`**: 数据层。封装了 GitHub REST API 的调用（GET 获取文件列表, PUT 上传文件, DELETE 删除文件），处理认证和配置存储。
*   **`Particles`**: 负责生成和更新构成圣诞树的金色粒子系统。
*   **`Snow` / `Star`**: 装饰性环境元素。

---

## 📖 操作指南 (User Guide)

### 1. 界面按钮
位于屏幕底部的控制栏：
*   **预览开关**: 显示/隐藏左下角的摄像头手势识别预览窗。
*   **上传**: 选择本地图片上传到 3D 场景（若配置了 GitHub，将自动上传到云端）。
*   **管理/清除**: 打开照片管理面板。
    *   **小绿点/灰点**: 指示存储状态（绿色=已连接 GitHub，灰色=仅本地模式）。
*   **静音**: 开启或关闭背景音乐。

### 2. 配置云存储 (GitHub Integration)
为了让大家（包括你自己）上传的照片能永久保存并在不同设备间同步，需要配置 GitHub 仓库信息。

1.  点击底部的 **管理 (📁)** 按钮，打开面板，点击右上角的 **“⚙️ 设置”**。
2.  填写以下信息：
    *   **用户名 (Owner)**: GitHub 用户名。
    *   **仓库名 (Repo)**: 用于存储照片的仓库名称。
    *   **文件夹 (Path)**: 照片存放路径（默认 `photos/`）。
    *   **Token**: 具有 `Contents: Read and write` 权限的 GitHub Personal Access Token (PAT)。

### 3. 注意事项
*   **浏览器权限**: 首次打开需要允许**摄像头**权限以启用手势控制，允许**音频**权限以播放背景音乐。
*   **性能**: 3D 特效和 AI 识别需要一定的 GPU 性能，建议使用 Chrome 或 Edge 浏览器。

---

## 🚀 部署流程 (Deployment)

本项目完全是一个静态网页，可以部署在任何静态托管服务上（GitHub Pages, Vercel, Netlify 等）。

### 部署到 GitHub Pages (推荐)

1.  将 `index.html` 提交到 GitHub 仓库。
进入仓库 **Settings** -> **Pages**。
2.  在**Activions** -> **New Workflow** 粘贴 `deploy.yml` 代码。
3.  在 **Build and deployment** 下，选择 **Source** 为 `Deploy from Action`。
4.  选择 `main` (或 `master`) 分支作为发布源。
5.  保存后，GitHub Action 会自动构建，几分钟后即可通过 `<username>.github.io/<repo>` 访问。

### 配置 Token 安全 (高级)
为了避免用户每次都在网页上手动输入 Token，可以通过 GitHub Actions 在构建时注入非敏感的配置（如 Repo 名），但**强烈建议 Token 仍由用户端输入**以防泄露。

*本项目当前逻辑设计为：HTML 中不硬编码 Token，完全依赖用户在浏览器本地存储 (`localStorage`) 中保存 Token，确保安全性。*
---

祝你玩得开心！Merry Christmas! 🎅🎁
