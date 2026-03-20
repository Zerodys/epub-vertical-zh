# AGENTS.md — epub-vertical-zh

繁體中文豎排 EPUB 格式定義與完整範本。

## 📦 Build Commands

### 打包 EPUB（標準方式）
```bash
cd epub-vertical-zh
zip -X -0 ../epub-vertical-zh.epub mimetype
zip -X -r ../epub-vertical-zh.epub META-INF OEBPS
```

### 驗證 EPUB（EPUBCheck）
```bash
# 安裝後使用
java -jar epubcheck.jar epub-vertical-zh.epub -mode exp

# 或指定模式
java -jar epubcheck.jar epub-vertical-zh.epub -mode mo -mode exp
```

### Calibre 轉換為 Kindle
```bash
# CLI 完整命令
ebook-convert epub-vertical-zh.epub epub-vertical-zh.azw3 \
    --output-profile kindle \
    --mobi-file-type both \
    --embed-all-fonts \
    --subset-embedded-fonts \
    --extra-css '.vrtl{writing-mode:vertical-rl}.hltr{writing-mode:horizontal-tb}' \
    --language zh-TW \
    --disable-font-rescaling \
    --expand-css
```

### KindleGen 替代方案
```bash
kindlegen epub-vertical-zh.epub -c1 -verbose
```

## 📁 Project Structure

```
epub-vertical-zh/
├── mimetype                              # 容器格式識別（不壓縮，必須首位）
├── META-INF/
│   └── container.xml                      # 指向 content.opf
└── OEBPS/
    ├── content.opf                       # 包裝文檔（元數據 + manifest + spine）
    ├── nav.xhtml                         # EPUB 3 導航文檔
    ├── toc.ncx                           # EPUB 2 向後兼容目錄
    ├── styles/
    │   └── vertical.css                  # 核心樣式表
    ├── fonts/                            # 嵌入字體
    ├── images/                           # 圖片資源
    └── xhtml/                            # 內容文檔
```

## 🎨 Code Style Guidelines

### XHTML 結構
- 使用 XHTML 1.1 語法（`<?xml version="1.0" encoding="UTF-8"?>`）
- 必備命名空間：`xmlns="http://www.w3.org/1999/xhtml"` 和 `xmlns:epub="http://www.idpf.org/2007/ops"`
- 必備屬性：`lang="zh-Hant"` 和 `dir="rtl"`
- 每個章節用 `<section epub:type="chapter">` 包裹
- 章節內用 `<section epub:type="level1">` 標記層級
- `<title>` 和 `<meta charset="UTF-8"/>` 不可省略

### CSS 書寫模式（核心）
```css
/* 豎排：必備全部前綴（順序：epub → webkit → ms → 標準） */
html, body {
    -epub-writing-mode: vertical-rl;
    -webkit-writing-mode: vertical-rl;
    -ms-writing-mode: tb-rl;
    writing-mode: vertical-rl;
    
    -epub-text-orientation: upright;
    -webkit-text-orientation: upright;
    text-orientation: upright;
}
```

### CSS 屬性順序
1. `-epub-*` 前綴屬性
2. `-webkit-*` 前綴屬性
3. `-ms-*` 前綴屬性
4. 標準屬性
5. 字體相關（font-family, font-size, line-height）
6. 文本對齊（text-align, text-indent）
7. 邊距和填充（margin, padding）

### Vendor Prefixes 順序
```css
/* 始終按此順序排列前綴 */
-epub-xxx: value;      /* EPUB 標準 */
-webkit-xxx: value;    /* Apple / Google Play / Android */
-ms-xxx: value;        /* IE / 舊版 Edge */
xxx: value;            /* 標準（W3C） */
```

### 命名規範
| 類型 | 規範 | 示例 |
|------|------|------|
| CSS 類名 | kebab-case | `.vertical-note`, `.note-text` |
| ID | camelCase（駝峰式） | `id="chapter01"`, `id="s1-1"` |
| epub:type | 冒號分隔 | `epub:type="chapter"` |
| 檔案名 | 與 manifest id 對應 | `chapter01.xhtml` → `id="chapter01"` |

### 雙行夾注 HTML 結構
```html
<div class="vertical-note">
    <span class="note-text">注釋（小字紅色，寬度 45%）</span>
    <span class="main-text">正文（大字黑色，flex: 1）</span>
</div>
```

### Ruby 注音格式
```html
<!-- 繁體注音 -->
<ruby>繁體<rt>ㄈㄢˊ ㄊㄧˇ</rt></ruby>

<!-- 日文振假名 -->
<ruby>東京<rt>とうきょう</rt></ruby>
```

### 圖片與表格
- 圖片、表格必須覆蓋為橫排：
```css
img, table {
    -webkit-writing-mode: horizontal-tb;
    writing-mode: horizontal-tb;
}
```

### 分頁控制
```css
h1, h2, h3, h4, h5, h6 {
    page-break-before: avoid;
    break-before: avoid;
    page-break-after: always;
}

p {
    page-break-inside: avoid;
    orphans: 2;
    widows: 2;
}
```

### 段落首行縮排
```css
p {
    text-indent: 2em;  /* 兩個全角字寬 */
}
```

### 檔案路徑（相對於 CSS）
```css
@font-face {
    src: url(../fonts/NotoSerifTC-Regular.otf) format("opentype");
}

img {
    src: url(../images/cover.png);
}
```

## ⚠️ Important Rules

1. **`mimetype` 必須不壓縮，放在 ZIP 第一位**
2. **所有 XHTML 必須 UTF-8 編碼**
3. **content.opf 的 `page-progression-direction="rtl"` 不可省略**
4. **manifest 的 `id` 必須與檔案名一致**
5. **Kindle 不支援 `flex`，雙行夾注需使用 float fallback**
6. **Calibre 會刪除 `-webkit-writing-mode`，需用 Extra CSS 注入**

## 📚 Resources

- [EPUB 3.3 (W3C)](https://www.w3.org/TR/epub-33/)
- [CSS Writing Modes Level 3](https://www.w3.org/TR/css-writing-modes-3/)
- [Apple Books Asset Guide](https://help.apple.com/itc/booksassetguide/)
- [Readium CSS — CJK Vertical](https://github.com/edrlab/thorium-reader/tree/develop/src/resources/ReadiumCSS)
