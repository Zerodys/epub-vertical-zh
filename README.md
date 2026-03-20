# epub-vertical-zh / 繁體中文豎排 EPUB 格式定義

> 繁體中文豎排 EPUB 格式定義與完整範本 — 支援 CSS Writing Modes、Ruby 注音、Tatechuyoko、Vendor Prefixes 最佳實踐

## 📖 範本結構

```
epub-vertical-zh/
├── mimetype                              # EPUB 容器格式識別（不壓縮）
├── META-INF/
│   └── container.xml                      # 指向 content.opf 的引導檔案
└── OEBPS/
    ├── content.opf                       # 包裝文檔（元數據 + manifest + spine）
    ├── nav.xhtml                         # EPUB 3 導航文檔（目錄）
    ├── toc.ncx                           # EPUB 2 向後兼容目錄
    ├── styles/
    │   └── vertical.css                  # 豎排樣式表（核心）
    ├── fonts/                            # 嵌入字體目錄
    ├── images/
    │   └── cover.png                     # 封面圖片
    └── xhtml/
        ├── cover.xhtml                  # 封面（SVG 格式）
        ├── titlepage.xhtml               # 書名頁
        ├── chapter01.xhtml               # 第一章：概述
        └── chapter02.xhtml              # 第二章：方法
```

## ✅ 已實現功能

| 功能 | 說明 | 支援狀態 |
|------|------|----------|
| `writing-mode: vertical-rl` | 豎排書寫模式 | ✅ |
| Vendor Prefixes | `-epub-`, `-webkit-`, `-ms-` 全系列 | ✅ |
| `text-orientation: upright` | 字符直立顯示 | ✅ |
| `text-combine-upright` | Tatechuyoko 橫中橫（數字/英文） | ✅ |
| `ruby` 注音 | 繁體注音、振假名 | ✅ |
| `page-progression-direction="rtl"` | 從右至左翻頁 | ✅ |
| `dir="rtl"` + `xml:lang="zh-Hant"` | 繁體中文語言標記 | ✅ |
| **雙行夾注（側注）** | `flex` 橫排＋各自豎排，小字紅色 | ✅ |
| 圖片橫排處理 | 自動覆蓋為 `horizontal-tb` | ✅ |
| 分頁控制 | 避免章節/段落中途截斷 | ✅ |
| 嵌入字體 | `@font-face` 模板 | ✅ |
| `epub:type` 語義結構 | 章節、段落標題語義化 | ✅ |
| SVG 封面 | 響應式、設備無關封面 | ✅ |
| EPUB 2 向後兼容 | NCX 目錄 | ✅ |

## 🖥 閱讀器兼容性

| 閱讀器 | 支援 | 說明 |
|--------|------|------|
| Apple Books (iOS/macOS) | ✅ | `-webkit-writing-mode` |
| Google Play Books | ✅ | 完整支援 |
| Calibre Viewer | ✅ | 完整支援 |
| Thorium Reader | ✅ | Readium CSS 標準 |
| Amazon Kindle | ⚠️ | 需 Calibre 轉換 |
| KOReader | ⚠️ | 部分支援 |

## 🚀 使用方式

### 1. 作為 EPUB 範本直接使用

```bash
# 將此專案 ZIP 打包為 EPUB
cd epub-vertical-zh
zip -X -0 ../epub-vertical-zh.epub mimetype
zip -X -r ../epub-vertical-zh.epub META-INF OEBPS
```

> ⚠️ 注意：`mimetype` 必須 **不壓縮**，放在 ZIP 最前面。

### 2. 作為格式定義參考

閱讀以下關鍵檔案：

- `OEBPS/content.opf` — EPUB 3 元數據結構、spine、page-progression-direction
- `OEBPS/styles/vertical.css` — 完整的 CSS 豎排樣式表
- `OEBPS/nav.xhtml` — EPUB 3 導航文檔
- `OEBPS/toc.ncx` — EPUB 2 向後兼容目錄

### 3. 嵌入自定義字體

```css
/* 在 vertical.css 中取消注釋並修改路徑 */
@font-face {
    font-family: "Noto Serif CJK TC";
    font-weight: normal;
    font-style: normal;
    src: url(../fonts/NotoSerifTC-Regular.otf) format("opentype");
    -epub-font-format: opentype;
}
```

### 4. 使用 EPUBCheck 驗證

```bash
java -jar epubcheck.jar epub-vertical-zh.epub -mode exp
```

## 📚 核心 CSS 屬性速查

```css
/* 豎排模式 — 全部前綴 */
html {
    -epub-writing-mode: vertical-rl;    /* EPUB 標準 */
    -webkit-writing-mode: vertical-rl; /* Apple / Google */
    -ms-writing-mode: tb-rl;            /* IE */
    writing-mode: vertical-rl;          /* 標準 */
}

/* 字符方向 */
text-orientation: upright;              /* 全部直立 */
text-orientation: mixed;                /* 標點旋轉 */

/* 數字橫排（Tatechuyoko） */
text-combine-upright: digits 4;
-webkit-text-combine-upright: digits 4;
```

## 🔖 相關資源

- [EPUB 3.3 (W3C Recommendation)](https://www.w3.org/TR/epub-33/)
- [CSS Writing Modes Level 3](https://www.w3.org/TR/css-writing-modes-3/)
- [Apple Books Asset Guide](https://help.apple.com/itc/booksassetguide/)
- [Readium CSS — CJK Vertical](https://github.com/edrlab/thorium-reader/tree/develop/src/resources/ReadiumCSS)
- [Noto Serif CJK — Google Fonts](https://fonts.google.com/noto/use)
- [思源宋體 (Source Han Serif) — Adobe Fonts](https://github.com/adobe-fonts/source-han-serif)

## 📄 授權

本專案採用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 授權。
