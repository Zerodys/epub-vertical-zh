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
| **注釋彈窗（:target）** | CSS :target 弹窗，纯 CSS 无 JS，竖排定位 | ✅ |
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
- [KDP HTML/CSS 支援（KF8）](https://kdp.amazon.com/en_US/help/topic/GG5R7N649LECKP7U)
- [KDP Enhanced Typesetting 屬性](https://kdp.amazon.com/en_US/help/topic/GB5GDY7WAJDN9GFK)
- [Calibre 使用手冊](https://manual.calibre-ebook.com/conversion.html)

## 📱 Kindle 轉換指南

> 以下設定專為將本範本轉換至 Amazon Kindle 設備優化。

### Kindle CSS 支援速查

| 功能 | 支援 | 說明 |
|------|------|------|
| `writing-mode: vertical-rl` | ✅ 有條件 | 需啟用 Enhanced Typesetting |
| `-webkit-text-orientation` | ✅ | mixed / upright / sideways |
| `-webkit-writing-mode` | ⚠️ 需 Extra CSS | Calibre 會刪除此屬性 |
| `-epub-writing-mode` | ⚠️ 需 Extra CSS | Calibre 會刪除此屬性 |
| `display: flex` | ⚠️ 不穩定 | 建議用 float/block 代替 |
| `<ruby>` / `<rt>` | ✅ | KF8 Enhanced Typesetting |
| `@font-face` 嵌入 | ✅ | 僅 AZW3（不用 MOBI） |

### ⚠️ Calibre Bug #1862447

Calibre 在轉換時會刪除 `-webkit-writing-mode` 和 `-epub-writing-mode` 屬性（被標記為 Unknown Property）。

**解決方案**：在轉換設定中透過 Extra CSS 注入豎排宣告。

### Calibre GUI 轉換步驟

1. 打開 Calibre → **新增書籍** → 選擇本 EPUB 檔案
2. 選中書籍 → 右鍵 → **轉換書籍** → **個別轉換**
3. **輸出格式**：選擇 `AZW3`（❌ 不要選 MOBI，Mobi 不支援豎排）
4. **外觀 → 字體**：
   - ✅ 啟用「**嵌入所有字體**」
   - ✅ 啟用「**壓縮嵌入字體**」（CJK 字體通常 5-17MB，必須壓縮）
5. **外觀 → 樣式 → Extra CSS**（在最下方）：
  粘貼以下內容：

```css
.vrtl {
    writing-mode: vertical-rl;
}
.hltr {
    writing-mode: horizontal-tb;
}
.vn-note-kindle {
    writing-mode: vertical-rl;
    display: block;
    float: right;
    clear: right;
    margin-right: 0.5em;
    font-size: 0.55em;
    color: #c0392b;
    line-height: 1.6;
}
.vn-main-kindle {
    writing-mode: vertical-rl;
    display: block;
    font-size: 1em;
    color: #1a1a1a;
}
.tcy-kindle {
    display: inline;
    text-orientation: mixed;
    writing-mode: horizontal-tb;
}
ruby { ruby-position: over; }
rt { font-size: 0.5em; }
```

6. **頁面設定 → 輸出設定檔**：選擇 `Kindle`（或 Kindle Paperwhite / Kindle Scribe）
7. 點擊「**確定**」開始轉換

### Calibre CLI 完整命令

```bash
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

| 參數 | 用途 |
|------|------|
| `--output-profile kindle` | 針對 Kindle 優化 |
| `--mobi-file-type both` | 生成 MOBI6+KF8 混合檔 |
| `--embed-all-fonts` | 嵌入所有字體 |
| `--subset-embedded-fonts` | 壓縮字體（必須，CJK 字體很大） |
| `--extra-css` | 注入 writing-mode（繞過 Calibre Bug） |
| `--expand-css` | 展開 CSS（更好的 Kindle 兼容性） |
| `--disable-font-rescaling` | 保留原始字體大小 |
| `--language zh-TW` | 設置繁體中文元數據 |

### KindleGen 替代方案（更好的豎排支援）

若 Calibre 轉換效果不理想，可使用 Amazon 官方的 KindleGen：

```bash
kindlegen epub-vertical-zh.epub -c1 -verbose
```

KindleGen 不會刪除 `-webkit-writing-mode`，豎排支援更好。

### Kindle 設備測試清單

- [ ] Kindle Paperwhite（第 5 代以上）
- [ ] Kindle Scribe
- [ ] Kindle Android/iOS App
- [ ] Kindle Previewer 3（桌面模擬器）

測試項目：
1. ✅ 豎排文字是否從右至左顯示
2. ✅ 雙行夾注是否對齊（左注右文）
3. ✅ 數字是否橫排顯示
4. ✅ 字體是否正確嵌入（不是系統預設字體）
5. ✅ Ruby 注音位置是否正確

### Kindle Enhanced Typesetting 說明

Amazon 在 2016 年為 Kindle 推出了 Enhanced Typesetting（增強排版），支援：
- `writing-mode: vertical-rl`（豎排）
- `-webkit-text-orientation: upright`
- Ruby 注音定位
- 高級連字符

**在 KDP 上傳書籍時，Enhanced Typesetting 會自動啟用**（若書籍通過檢查）。

### 參考文獻

- [Calibre Bug #1862447: writing-mode 屬性被刪除](https://bugs.launchpad.net/calibre/+bug/1862447)
- [Kenrick's Notes: 用 Calibre 轉換日文 Kindle 書籍](https://blog.kenrick95.org/2024/08/converting-japanese-e-book-in-kindle-with-calibre/)
- [KDP Enhanced Typesetting 屬性和標籤](https://kdp.amazon.com/en_US/help/topic/GB5GDY7WAJDN9GFK)

## 📄 授權

本專案採用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 授權。
