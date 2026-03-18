---
name: markitdown
description: Convert files and office documents to Markdown. Supports PDF, DOCX, PPTX, XLSX, images (with OCR), audio (with transcription), HTML, CSV, JSON, XML, ZIP, YouTube URLs, EPubs and more. Use when converting documents to markdown, extracting text from PDFs, processing office files, or when user mentions document conversion, PDF to markdown, or file extraction.
---

# MarkItDown - File to Markdown Conversion

## Overview

MarkItDown is a Python tool developed by Microsoft for converting various file formats to Markdown. It's particularly useful for converting documents into LLM-friendly text format, as Markdown is token-efficient and well-understood by modern language models.

**Key Benefits**:
- Convert documents to clean, structured Markdown
- Token-efficient format for LLM processing
- Supports 15+ file formats
- Optional AI-enhanced image descriptions
- OCR for images and scanned documents
- Speech transcription for audio files

## Supported Formats

| Format | Description | Notes |
|--------|-------------|-------|
| **PDF** | Portable Document Format | Full text extraction |
| **DOCX** | Microsoft Word | Tables, formatting preserved |
| **PPTX** | PowerPoint | Slides with notes |
| **XLSX** | Excel spreadsheets | Tables and data |
| **Images** | JPEG, PNG, GIF, WebP | EXIF metadata + OCR |
| **Audio** | WAV, MP3 | Metadata + transcription |
| **HTML** | Web pages | Clean conversion |
| **CSV** | Comma-separated values | Table format |
| **JSON** | JSON data | Structured representation |
| **XML** | XML documents | Structured format |
| **ZIP** | Archive files | Iterates contents |
| **EPUB** | E-books | Full text extraction |
| **YouTube** | Video URLs | Fetch transcriptions |

## Installation

```bash
# Install with all features
pip install 'markitdown[all]'

# Or install specific formats
pip install 'markitdown[pdf, docx, pptx]'
```

## Command-Line Usage

```bash
# Basic conversion
markitdown document.pdf > output.md

# Specify output file
markitdown document.pdf -o output.md

# Pipe content
cat document.pdf | markitdown > output.md
```

## Python API

```python
from markitdown import MarkItDown

# Basic usage
md = MarkItDown()
result = md.convert("document.pdf")
print(result.text_content)

# Convert from stream
with open("document.pdf", "rb") as f:
    result = md.convert_stream(f, file_extension=".pdf")
    print(result.text_content)
```

## AI-Enhanced Image Descriptions

Use LLMs via OpenRouter to generate detailed image descriptions:

```python
from markitdown import MarkItDown
from openai import OpenAI

client = OpenAI(
    api_key="your-openrouter-api-key",
    base_url="https://openrouter.ai/api/v1"
)

md = MarkItDown(
    llm_client=client,
    llm_model="anthropic/claude-sonnet-4.5",
    llm_prompt="Describe this image in detail"
)

result = md.convert("presentation.pptx")
print(result.text_content)
```

## Common Use Cases

### Convert Scientific Papers

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("research_paper.pdf")
with open("paper.md", "w") as f:
    f.write(result.text_content)
```

### Extract Data from Excel

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("data.xlsx")
print(result.text_content)  # Markdown table format
```

### YouTube Transcription

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("https://www.youtube.com/watch?v=VIDEO_ID")
print(result.text_content)
```

## Troubleshooting

**Missing dependencies:**
```bash
pip install 'markitdown[pdf]'  # For PDF support
```

**OCR not working:**
```bash
# macOS
brew install tesseract

# Ubuntu
sudo apt-get install tesseract-ocr
```

## Resources

- **GitHub**: https://github.com/microsoft/markitdown
- **PyPI**: https://pypi.org/project/markitdown/
- **OpenRouter**: https://openrouter.ai (for AI-enhanced conversions)
