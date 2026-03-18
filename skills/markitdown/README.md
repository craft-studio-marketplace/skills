# markitdown

Convert files and office documents to Markdown using Microsoft's MarkItDown library. Supports 15+ formats including PDF, DOCX, PPTX, XLSX, images (with OCR), audio (with transcription), HTML, CSV, JSON, XML, ZIP, YouTube URLs, and EPubs.

> **Note:** For PDFs with tables or complex layouts, `opendataloader-pdf` offers significantly better accuracy (benchmark: 0.90 vs. 0.29). Use markitdown for non-PDF formats or simple text PDFs.

## Installation

```bash
git clone https://github.com/craft-studio-marketplace/skills.git craft-studio-skills
cp -r craft-studio-skills/skills/markitdown ~/.claude/skills/
```

Requires:
```bash
pip install 'markitdown[all]'
```

## Usage

```
/markitdown
```

Then provide the path to the file you want to convert.

## Supported Formats

PDF, DOCX, PPTX, XLSX, JPEG, PNG, GIF, WebP, WAV, MP3, HTML, CSV, JSON, XML, ZIP, EPUB, YouTube URLs

## License

MIT
