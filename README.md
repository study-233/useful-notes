# 工具与笔记

一个用 Markdown 维护的个人知识库，收录实用工具、优质网站和教程。

- 网站：<https://study-233.github.io/useful-notes/>
- 技术：MkDocs + Material for MkDocs
- 发布：提交到 `main` 后，由 GitHub Actions 构建并发布到 GitHub Pages

## 在本地预览

需要 Python 3.12 或更新版本。在项目目录打开 PowerShell：

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m mkdocs serve --dev-addr 127.0.0.1:8765
```

打开终端打印的网址；保存 Markdown 后，预览会自动更新。已经建立环境后，日常只需要运行最后一条命令。

macOS / Linux 对应使用 `.venv/bin/python`。

## 内容放在哪里

| 内容 | 文件 |
| --- | --- |
| 首页分类与快捷入口 | `docs/index.md` |
| 网页翻译工具 | `docs/tools/translation.md` |
| Zotero 插件 | `docs/tools/zotero.md` |
| 网站收藏 | `docs/websites.md` |
| 教程列表 | `docs/tutorials/index.md` |
| ChatGPT 订阅教程 | `docs/tutorials/chatgpt-subscription.md` |
| 网站名称、主题、导航 | `mkdocs.yml` |
| 少量自定义样式 | `docs/assets/stylesheets/extra.css` |

`docs/` 内的内容会发布到网站。`templates/` 是复制用的模板，不会出现在网站和搜索结果中。

## 添加工具或网址

1. 打开相应分类页。
2. 从 `templates/resource-card.md` 复制一张卡片，放进页面的 `<div class="grid cards" markdown>` 内。
3. 填写名称、简介、用途和完整网址。列表项后续段落保留四个空格的缩进。
4. 如果希望首页也能直接打开它，在 `docs/index.md` 的对应卡片内补充一行链接。

外部网址保留原始参数和 `#` 后的筛选内容。例如插件商店的 `#tags=favorite` 不应删除。

## 添加教程

1. 复制 `templates/tutorial.md` 到 `docs/tutorials/`，使用简短英文文件名，例如 `zotero-backup.md`。
2. 替换标题、简介和正文。使用 `##`、`###` 标题，右侧文章目录会自动生成。
3. 在 `docs/tutorials/index.md` 加一张教程卡片，链接写为 `zotero-backup.md`。
4. 在 `mkdocs.yml` 的 `nav` → “教程与学习”下添加：

```yaml
nav:
  # 保留其他已有分类，在现有“教程与学习”下追加最后一项。
  - 教程与学习:
      - 学习资源: tutorials/index.md
      - ChatGPT 订阅教程: tutorials/chatgpt-subscription.md
      - Zotero 备份教程: tutorials/zotero-backup.md
```

其他页面链接到这篇教程时，使用相对于当前 Markdown 文件的路径，例如首页使用 `tutorials/zotero-backup.md`。这样本地预览和 GitHub Pages 的仓库子路径都能正常工作。

## 插入图片和代码

将图片放在 `docs/assets/images/`。例如，在教程文章中写：

```markdown
![描述图片内容](../assets/images/example.png)
```

代码用三个反引号包围，并注明语言。页面会提供代码复制按钮：

````markdown
```powershell
python --version
```
````

## 调整分类和站名

分类导航在 `mkdocs.yml` 的 `nav` 中维护。新增分类时创建对应 Markdown 页面，再添加导航项，并同步首页的分类入口。

修改 `site_name` 可以调整站名；也请同步首页标题和这份说明。如果更换仓库或域名，同时更新 `site_url`、`repo_url` 和 `repo_name`。`site_url` 必须包含仓库子路径，并以 `/` 结尾。

## 验证与发布

提交前运行：

```powershell
.\.venv\Scripts\python.exe -m mkdocs build --strict
```

构建成功后，提交并推送：

```powershell
git add docs mkdocs.yml
git commit -m "Update notes"
git push origin main
```

首次启用时，在 GitHub 仓库 **Settings → Pages → Build and deployment → Source** 选择 **GitHub Actions**。仓库默认公开，内容提交到 `main` 后自动发布，也可在 Actions 页面手动运行工作流。

Pull Request 只运行构建检查，不发布。构建失败时，本次内容不会发布；在仓库 Actions 页面查看失败步骤并修复。需要恢复旧内容时，撤销对应提交再推送即可。

## 依赖更新

`requirements.txt` 固定构建依赖，包含用于中文搜索分词的 `jieba`。升级依赖后先本地构建并验证中英文搜索，再更新版本锁定文件和提交。
