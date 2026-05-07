# Smart Campus Map 项目说明

Smart Campus Map 是一个面向 XEC / TC 校园场景的交互式校园活动地图。它把公共活动、校园地点、学院筛选、Personal Space、个人提醒、打卡反馈、邮件提取和 Admin 发布流程放在同一张地图里，目标是让学生快速理解“活动在哪里、什么时候发生、适合谁、我需要做什么”。

在线版本：

https://ent208-group-9.github.io/smp_v2/

## 1. 项目定位

本项目不是普通活动列表，也不是单纯的静态校园地图。它的核心是“公共地图 + 个性化图层”：

- Public Map 显示 Admin 发布的校园公共活动和地点。
- Personal Space 是学生自己的个性化地图模式，不是 Student ID，也不是登录身份。
- 学生可以在 Personal Space 中继续查看公共事件，同时叠加自己的个人事件、提醒、打卡状态和邮件提取任务。
- 公共事件不会被复制成私人事件，学生对公共事件的提醒和打卡只影响自己的 Personal Space。
- Admin 只能发布公共活动到 Campus 或具体学院范围，不能发布到 Collection。

## 2. 技术结构

- React：构建学生端地图、Personal Space、SMART Agent 和 Admin 页面。
- Vite：本地开发、构建和 GitHub Pages 部署。
- localStorage：保存演示数据、个人空间、个人事件、提醒和打卡反馈。
- GitHub Pages：静态网页部署。
- 可选 Agent API：如果部署环境提供 `/api/agent-search`，SMART Agent 可以调用后端模型；没有后端时使用本地规则作为 demo fallback。

常用命令：

```bash
npm install
npm run dev
npm run build
npm run lint
```

本地开发地址通常为：

```text
http://127.0.0.1:5174/smp_v2/
```

## 3. 学生端主要界面

学生端默认进入 Public Map。页面主要由以下区域组成：

- 顶部操作区：Personal Space、语言切换、SMART Agent、Admin 入口。
- 时间筛选：Today、Next 7 days、Next 30 days、All dates。
- 搜索框：搜索活动、地点、楼栋、组织者或自然语言问题。
- 范围筛选：Collection、Campus 和各学院。
- Theme 筛选：Academic、Forum、Careers、Exhibition、Student life、Festival、Sports、Exam、Deadline。
- 推荐区：根据当前筛选展示接下来值得关注的活动、最近地点和当前范围推荐。
- 地图区：展示校园地图、地点、公共事件、个人事件、当前位置和聚合 marker。
- 详情栏：展示地点、活动、提醒、打卡和 Personal Space 相关信息，可折叠。

## 4. 范围筛选逻辑

### Collection / 集合

Collection 是“查看全部”的集合入口，不是具体发布范围。它用于把 Campus、学院活动和 Personal Space 中可见的个人内容一起浏览。

适合场景：

- 学生刚打开页面，还不确定要看哪个学院。
- 想同时看校级活动和学院活动。
- 想配合主题筛选，例如 Collection + Deadline。

### Campus / 校园

Campus 用于承载校级活动，例如：

- 面向全校学生的讲座。
- 校园节日、展览、开放活动。
- 全校范围的论坛、招聘或报名提醒。

Admin 发布校级活动时应选择 Campus。

### 学院范围

当前包含以下学院，界面支持中英双语：

- 智造生态学院 / IME
- 人工智能与先进计算学院 / AI & Advanced Computing
- 产金融合学院 / FinTech
- 智能机器人学院 / Robotics
- 物联网学院 / IoT
- 文化科技学院 / Culture Tech
- 芯片学院 / CHIPS

学院筛选只显示对应学院范围内的公共事件，同时 Personal Space 模式下可叠加个人事件。

## 5. Theme 主题分类

Theme 用于表达活动或个人事项的性质，并影响筛选、marker 图标和详情展示。

当前主题包括：

- Academic：学术、课程、讲座、研讨会。
- Forum：论坛、会议、panel、圆桌。
- Careers：招聘、就业、实习、行业活动。
- Exhibition：展览、展示、showcase。
- Student life：社团、学生生活、salon。
- Festival：节日、市集、嘉年华。
- Sports：运动、比赛、训练、体育活动。
- Exam：考试、测验、期中、期末。
- Deadline：截止日期、提交、报名截止。

主题同时用于学生筛选、Personal Space 个人事件、Admin 活动发布和 SMART Agent 邮件提取。

## 6. Public Map 默认模式

Public Map 是学生端默认模式。学生可以：

- 查看校园公共活动。
- 按时间、学院、Campus、Collection 和主题筛选。
- 搜索活动、地点、建筑、组织者。
- 点击地图 marker 查看地点或活动详情。
- 查看活动时间、地点、组织者、适合人群、说明和路线提示。

Public Map 只展示公共数据，不展示学生的个人任务。

## 7. Personal Space 模式

Personal Space 是个性化地图模式，不是上传页面，也不是登录系统。进入后学生仍然能看到公共事件，同时叠加自己的个性化内容。

Personal Space 可以保存：

- 个人事件，例如课程提醒、meeting、考试、deadline。
- 从邮件或通知中提取出的个人任务。
- 对公共事件设置的个人提醒。
- 对公共事件的打卡状态和反馈。

这些数据只属于当前浏览器中的当前 Personal Space，不会影响 Admin 数据，也不会让其他学生看到。

## 8. Personal Space 具体功能

### 添加个人事件

学生可以手动创建个人事件，选择标题、时间、地点、主题和说明。个人事件会以不同于公共事件的图标显示在地图上。

### 拖动个人事件位置

个人事件 marker 可以在地图上拖动。适用于邮件地点不清楚、临时会议没有公共地点、或者学生想微调任务位置的情况。

### 公共事件提醒

学生可以在 Personal Space 中对公共事件开启提醒。提醒是私人的，不会改变公共事件本身。

### 公共事件打卡与反馈

学生可以对公共事件进行 Check in。打卡后：

- 公共事件仍然留在地图上。
- marker 会用个人状态颜色区分“我已打卡”。
- 学生可以保存自己的反馈或后续记录。

### AI 邮件提取

Personal Space 中有 AI extract from email 区域。流程如下：

1. 粘贴转发邮件或通知文本。
2. 点击 Extract editable draft。
3. 系统提取标题、时间、地点、主题、说明和初始坐标。
4. 学生检查并修改草稿。
5. 保存后生成个人事件，显示在 Personal Space 地图图层中。

当前静态网页版本以本地规则提取为主，适合作为 demo 和原型。后续可接入真实模型服务来提高提取准确率。

## 9. SMART Agent

SMART Agent 位于语言切换旁边，进入聊天后会变成浮动对话窗口。它有两个核心能力：

- 自由问答：回答关于当前地图、活动、地点、路线和 Personal Space 的问题。
- 创建个人任务：把邮件、通知或自然语言描述整理成可编辑的个人事件草稿。

重要边界：

- SMART Agent 可以帮助添加 personal event。
- SMART Agent 不能创建、修改或发布 Admin 公共事件。
- SMART Agent 不能把学生的私人任务发布给所有人。

## 10. 地图与 marker 设计

地图是学生端核心体验。当前版本做了以下优化：

- 地图图片自适应窗口，减少右侧空白和偏移。
- 右侧详情栏可折叠，避免压缩地图。
- 移动端详情栏默认以 Peek 状态出现，优先露出地图。
- 多个相近事件会聚合为 cluster marker，点击后展开事件列表。
- marker 使用半透明背景，减少对底图文字和路线的遮挡。
- 公共事件、个人事件、已打卡事件使用不同视觉状态区分。

## 11. 推荐区

推荐区用于快速判断当前筛选下最值得关注的信息。标题会根据当前时间范围动态变化，避免 Today 为 0 时仍显示 “Recommended now” 的误导。

推荐卡片包括：

- Next up：当前筛选下的下一场活动。
- Nearest：距离当前位置或默认点最近的地点。
- For this scope：当前范围内的重点活动。

## 12. 定位与朝向

地图左侧有可收缩的定位浮标。它可以：

- 开启或关闭实时定位。
- 显示当前位置。
- 显示朝向。
- 根据校园地点给出距离感知提示。

如果浏览器拒绝定位或传感器权限，系统会使用默认锚点作为 fallback。

## 13. Admin 管理端

Admin 用于维护公共地图和公共活动。入口在学生端右上角，点击后会在新页面打开。

默认演示密码：

```text
123456
```

生产部署时建议通过环境变量设置密码：

```text
VITE_ADMIN_PASSWORD=your-password
```

Admin 可以：

- 新建公共活动。
- 修改活动标题、主题、组织者、时间、地点、范围和说明。
- 选择活动发布范围：Campus 或某个学院。
- 维护地点信息。
- 拖动地点 marker 校准坐标。
- 查看数据健康检查。

Admin 不应该把私人任务发布到公共地图。

## 14. 部署说明

本项目通过 GitHub Actions 部署到 GitHub Pages。部署配置位于：

```text
.github/workflows/deploy.yml
```

构建时会自动使用当前仓库名作为 Vite base path，因此同一套代码可以部署到：

- `/smart-campus-map/`
- `/smp_v2/`
- 其他 GitHub Pages 仓库路径

推送到 `main` 后，GitHub Actions 会自动构建并发布到 Pages。

## 15. 数据边界

当前版本是前端原型和静态部署版本，主要用于演示交互逻辑：

- 公共活动和地点数据保存在前端 demo 数据与本地存储中。
- Personal Space 数据保存在当前浏览器 localStorage 中。
- 换浏览器、换设备或清理缓存后，个人数据可能丢失。
- 如需多人真实使用，需要接入后端数据库、认证系统和真实 AI 服务。

## 16. 适合演示的用户流程

1. 打开线上页面。
2. 在 Public Map 中按时间、学院和主题筛选活动。
3. 点击地图 marker 查看地点与活动详情。
4. 进入 Personal Space。
5. 粘贴一封邮件或通知，提取个人事件草稿。
6. 修改标题、时间、地点和主题。
7. 保存后在地图上查看个人事件。
8. 对公共活动设置提醒或打卡反馈。
9. 打开 SMART Agent，询问地图、活动或让它整理个人任务。
10. 使用 Admin 页面演示公共活动发布流程。
