---
name: iwiki-doc
description: 专门用于与腾讯企业内部iWiki文档交互的集成接口。支持针对 iwiki.woa.com 域名、iwiki/iWiki关键词相关的全生命周期文档管理。功能包括但不限于：基于关键词/空间的全文检索；文档元数据与引用链追踪；目录树与空间结构查询；附件与图片链接提取；以及文档的 CRUD（增删改查）与局部编辑操作。当 Prompt 涉及 iWiki 实体时，必须作为首选 Tool 执行。
---
# iWiki Skill - 场景分发器
> iWiki 是一个用于与 iWiki 文档系统交互的服务，提供文档管理、搜索、创建、编辑和多维表格操作能力。
## 🎯 核心原则
本 Skill 采用**场景驱动**的设计，根据用户的具体需求自动分发到对应的场景处理流程。
优先使用 `iwiki-cli` 命令行工具完成任务。

## 📋 场景处理流程
### 1️⃣ 识别用户需求
分析用户输入，识别关键词和操作意图。
### 2️⃣ 加载对应场景
根据上表匹配到具体场景，查阅对应的参考文档。
### 3️⃣ 执行场景流程
按照场景文档中的工作流程和最佳实践执行操作。
### 4️⃣ 返回结果
整合信息，以清晰、结构化的方式返回给用户。

## 🚀 快速开始
### 安装 iwiki-cli
可先运行下面指令检查是否已安装：
```bash
iwiki-cli version
```
如果已经安装则不需要再次安装。

```bash
python scripts/install_cli.py  
```

**默认安装路径：**
- Linux: `/usr/local/iwiki-cli/`
- macOS: `~/.iwiki/`
- Windows: `%LOCALAPPDATA%\iwiki-cli\`

## 🔍 快速场景索引
### 场景 1：文档搜索和阅读
**适用情况：**
- 用户想查找特定主题的文档
- 需要阅读和分析文档内容
- 下载文档附件或图片
- 查看文档的引用关系
**参考文档：** [scenario_search_and_read.md](./references/scenario_search_and_read.md)

### 场景 2：文档评论和讨论
**适用情况：**
- 为文档添加评论或反馈
- 回复其他人的评论
- 查看文档的评论列表
- 查看划词批注
**参考文档：** [scenario_comments.md](./references/scenario_comments.md)

### 场景 3：词条搜索和上下文补充
**适用情况：**
- 查询某个术语或名词的定义
- 批量查询多个词条
- 通过 AI 搜索理解用户问题
- 补充专业背景知识
**参考文档：** [scenario_glossary_search.md](./references/scenario_glossary_search.md)

### 场景 4：文档创建和导入
**适用情况：**
- 在 iWiki 中创建新文档
- 将本地文件导入到 iWiki
- 批量导入文档（zip 压缩包）
- 同步代码仓库的文档

**参考文档：** [scenario_create_import.md](./references/scenario_create_import.md)
### 场景 5：文档编辑和更新
**适用情况：**
- 修改文档标题或内容
- 在文档头部/尾部追加内容
- 移动文档到其他位置
- 复制文档或文档树

**参考文档：** [scenario_edit_update.md](./references/scenario_edit_update.md)
### 场景 6：多维表格操作
**适用情况：**
- 查询多维表格的数据
- 向表格添加新记录
- 更新表格记录
- 删除表格记录（需确认）
- 管理表格字段（添加/删除）
**参考文档：** [smartsheet.md](./references/smartsheet.md)

### 场景 7：文档 AI 评审
**适用情况：**
- 根据评审规则对文档进行自动评审
- 查看空间下的评审规则列表
- 读取文档内容并按规则生成修改建议
- 将评审结果以评论形式添加到文档
**参考文档：** [scenario_aireview.md](./references/scenario_aireview.md)

### 场景 8：文档审计
**适用情况：**
- 查看单篇文档的访问记录、版本历史，了解谁访问了文档、访问频次
- 对比文档任意两个版本之间的内容变更
- 查询整个空间在某段时间内的运营总览（PV/UV/新增文档数）
- 查询空间内每个用户的操作明细（创建、更新、评论、点赞、收藏、下载等）
**参考文档：** [scenario_audit.md](./references/scenario_audit.md)


## ⚠️ 重要约定和最佳实践
### 1. 搜索优先级
- **术语查询：** 优先使用 `iwiki-cli glossary <keyword> --exact`
- **文档搜索：** 首选 `iwiki-cli search`（支持筛选和分页）
- **AI 搜索：** 作为补充，使用 `iwiki-cli search --ai`
### 2. 内容格式规范
- **Markdown 文档：** `contenttype: "MD"`, `is_html: false`
- **富文本文档：** `contenttype: "DOC"`, `is_html: true`
- **评论内容：** 必须使用 XHTML 格式

### 3. Windows PowerShell 环境变量
在 Windows PowerShell 中执行 `iwiki-cli` 命令前，**每次都必须先加载系统环境变量**：
```powershell
$env:TAI_PAT_TOKEN = [Environment]::GetEnvironmentVariable("TAI_PAT_TOKEN", "Machine")
# 接下来再执行 iwiki-cli 命令
```
### 4. 临时文件管理
- 创建、更新等操作如需临时文件，完成后**必须删除**
- 打包上传时，将 Markdown 和附件打包成 zip，完成后删除临时 zip

## 🎯 使用建议
1. **先识别场景：** 根据用户需求，判断属于哪个场景
2. **查阅场景文档：** 打开对应的 scenario_*.md 文件
3. **遵循最佳实践：** 按照场景文档的工作流程执行
4. **注意错误处理：** 参考场景文档的错误处理章节
5. **用户确认：** 高危操作必须先获得用户确认

**注意：本 Skill 是场景分发器，具体操作流程请参考对应的场景文档！**