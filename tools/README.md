# Tools

本目录维护 AI Agent 相关的实用工具，以 Git Submodule 方式引入。

## 已安装的工具

### MarkItDown

- **来源**：[microsoft/MarkItDown](https://github.com/microsoft/MarkItDown)
- **本地路径**：`tools/markitdown/`
- **版本**：v0.1.6
- **简介**：微软开源的轻量级文件转 Markdown 工具，将各种格式文件转为 Markdown 供 LLM 消费。保留标题、列表、表格、链接等文档结构，token 效率高。

#### 支持的格式

| 格式 | 说明 |
|------|------|
| PDF | PDF 文档 |
| PowerPoint | .pptx 演示文稿 |
| Word | .docx 文档 |
| Excel | .xlsx / .xls 表格 |
| Images | EXIF 元数据 + OCR |
| Audio | EXIF 元数据 + 语音转写 |
| HTML | 网页 |
| CSV / JSON / XML | 文本格式 |
| ZIP | 遍历压缩包内容 |
| YouTube | URL 转文字 |
| EPub | 电子书 |

#### 安装

```bash
# 安装全部依赖（推荐）
pip install 'markitdown[all]'

# 或按需安装
pip install 'markitdown[pdf,docx,pptx]'

# 或从源码安装
cd tools/markitdown
pip install -e 'packages/markitdown[all]'
```

#### 使用

**命令行**：

```bash
# 转换文件输出到 stdout
markitdown document.pdf

# 保存到文件
markitdown document.pdf -o document.md

# 管道方式
cat document.pdf | markitdown
```

**Python API**：

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("document.pdf")
print(result.text_content)
```

**可选依赖**：

| 安装选项 | 支持格式 |
|---------|---------|
| `[all]` | 全部格式 |
| `[pdf]` | PDF |
| `[docx]` | Word |
| `[pptx]` | PowerPoint |
| `[xlsx]` | Excel |
| `[xls]` | 旧版 Excel |
| `[outlook]` | Outlook 邮件 |
| `[audio-transcription]` | 音频转写 |
| `[youtube-transcription]` | YouTube 转写 |
| `[az-doc-intel]` | Azure Document Intelligence |
| `[az-content-understanding]` | Azure Content Understanding |

#### 与 Prometheus 集成

MarkItDown 可用于 Prometheus RAG Subagent 的文档摄入流水线，替代 pdfplumber + python-docx：

```
原始方案: pdfplumber 提取 PDF -> 分段 -> 嵌入
MarkItDown: markitdown 转换 PDF -> Markdown -> 分段 -> 嵌入

优势:
- 统一接口：一个工具处理所有格式（PDF/DOCX/PPTX/XLSX/HTML）
- 保留结构：标题/列表/表格/链接转为 Markdown 格式
- token 效率高：Markdown 比纯文本更省 token
```

## 更新 Submodule

```bash
# 更新 markitdown 到上游最新
cd tools/markitdown && git pull origin main && cd ../..
git add tools/markitdown
git commit -m "chore: update markitdown submodule"
```
