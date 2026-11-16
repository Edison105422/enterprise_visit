# 企业参访 SOP 离线执行台

## 背景与权威数据
- 用户要求在 /Users/apple/projects/enterprise_visit/ 下创建 SOP_dashboard.html，以当前 SOP.md 的实际任务、时间、编号与依赖为准，保留用户已有修改。
- 2026-11-16 快照 SHA-256：96fe4c09e56c246457cd1b6dd3db8677d764c9f73339ee450aa3d1968ee5fe51。
- 原文甘特图说明实际在第三节，完整数据在第四节，第六节是管理机制。
- 35 项任务、8 个岗位、80 条双向一致依赖。保留 Z08 招募组织、Z06 向部长汇报、P01 企业审核等原文细节。
- Z01 任务表为 T-21，首段及验收节点写启动不限时；初始按任务表 T-21，页面明确提示差异，支持改为不限时。

## 实现与数据
- 文件：/Users/apple/projects/enterprise_visit/SOP_dashboard.html。内联 CSS、JS、初始 JSON，无外部资源；可 file:// 直接打开。
- `script#seedData` 是 SOP 快照与初始 35 项任务；运行时 `state.tasks` 为两模块的唯一任务数据源。
- 每个 task 保留 id、owner、title、content、acceptance、upstream、downstream、originalTime，并有 executor、status、materials、reviewer、reviewResult、notes、timeNote。
- 排期结构：`schedule: {unit: 'day'|'hour'|'none', start, end}`。day 使用半开区间，end 为截止日 +1；UI 显示含截止日。单日 T-5 为 `{unit:'day',start:-5,end:-4}`；hour 起止严格递增；none 的 start/end 均为 null。
- 三轴：筹备 T<0，现场 h 小时，活动后日轴从 T=0 起；P03/P04/R03 在活动后轴从 T 开始。跨 T 的日级任务在筹备/活动后分段显示。
- 拖动条身与两端；日级 1 天、小时级 0.25 小时吸附；表单小时值可更精确。时间粒度切换时清空偏移，防止天和小时混用。
- 依赖用原文文本保存，编号形成有向图。增删依赖同步对方反向引用，改名同步标题并保留括号备注。阻止自依赖、未知编号、环路、引用不对称；阶段性交付/排期重叠仅提醒。已完成须验收通过且上游全部完成。

## 保存与导出
- localStorage key：`enterprise-visit-sop:/Users/apple/projects/enterprise_visit/SOP.md:v1`。
- 有效输入停止约 550ms 后自动提交，非法草稿不覆盖上一有效版本；有保存失败提示、同浏览器多窗口冲突提示、会话内最多 30 次撤销。
- JSON 导入校验全 35 个原编号、字段、版本、原文快照、时间和依赖；确认替换前尝试下载当前备份。JSON 导出含全部执行字段和 SOP 原文快照。
- Markdown 只导出当前任务清单、执行和验收信息及核对提醒，不重写原文其他章节。浏览器不会直接回写 SOP.md。
- 浏览器存储与浏览器/路径绑定；交接与换设备使用 JSON。HTML 不会自动读取未来修改的 SOP.md。

## 验证与后续维护
- 临时独立 Chrome headless，真实鼠标事件测试移动/两端缩放、字段同步、依赖更改与环校验、完成验收、JSON 导入导出、Markdown、file:// 刷新恢复、390px 手机布局、输入转义，共 37 项通过；核对桌面和手机截图。
- 初始嵌入数据与 SOP.md 逐字段一致；创建后核对源文件哈希未改动。
- 更新时先重新阅读最新 SOP.md。修改 HTML 初始任务数据与校验规则时同步检查原编号、source 快照/hash；已有 localStorage 不会自动迁移，需明确版本策略，避免覆盖执行记录。
- 本次工作中间资料保存在 /Users/apple/Documents/Codex/2026-11-16/referenced-chatgpt-conversation-this-is-an-2/work/，用户可下载副本在同工作目录 outputs/SOP_dashboard.html。
