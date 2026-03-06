# ScoreFlow

iOS 樂譜閱讀 App — 專為音樂家設計，解決演奏時翻頁不便的問題。

## Features

### 樂譜匯入與管理
- 支援 PDF 和圖片格式 (JPG/PNG)
- 從 Files app 匯入，Grid 縮圖瀏覽
- 搜尋、排序、分類管理

### 樂譜閱讀
- PDF 原生渲染 (PDFKit)
- 點擊 / 滑動翻頁
- 記住最後閱讀位置

### 自訂區域翻頁 (核心功能)
使用者可在樂譜上框選矩形區域，翻頁時只有該區域捲動顯示下一段，其餘部分保持不動。適合演奏時預覽下一段樂譜。

**技術原理：Snapshot + Mask + Scroll**
1. 進入「編輯區域」模式，拖曳畫矩形定義區域
2. 頁面載入時預渲染快照
3. 點擊區域時，在 clipping mask 內播放捲動動畫
4. 狀態機：正常 → 部分翻頁 → 完整翻頁

### 註記
- Apple Pencil / 手指手繪 (PencilKit)
- 文字方塊註記，可拖曳定位
- 自動儲存，跨裝置保持

### 曲目清單 (Setlist)
- 建立演出清單，拖曳排序
- 連續播放模式

## Tech Stack

| 技術 | 用途 |
|------|------|
| Swift / SwiftUI | UI 框架 (iOS 17+) |
| PDFKit | PDF 渲染 + `PDFPageOverlayViewProvider` |
| PencilKit | Apple Pencil 手繪註記 |
| SwiftData | 資料持久化 |

## Architecture

MVVM 架構，主要針對 iPad，同時支援 iPhone。

```
ScoreFlow/
├── App/                    # Entry point, ContentView
├── Models/                 # SwiftData @Model
│   ├── Score.swift          # 樂譜元資料
│   ├── PageRegion.swift     # 自訂翻頁區域 (normalized 座標)
│   ├── Annotation.swift     # 註記 (PencilKit / 文字)
│   ├── Setlist.swift        # 曲目清單
│   └── ScoreBookmark.swift  # 書籤
├── ViewModels/             # 狀態管理
├── Views/
│   ├── Library/             # 樂譜庫
│   ├── Viewer/              # 主閱讀畫面
│   ├── RegionTurn/          # 區域翻頁
│   ├── Annotations/         # 註記工具
│   └── Setlists/            # 曲目清單
├── Services/               # 檔案匯入、渲染、縮圖
└── Utilities/              # 工具擴展
```

## Milestones

| Phase | Milestone | 內容 |
|-------|-----------|------|
| 1 | 基礎建設 | Xcode 專案、SwiftData Models、檔案匯入、樂譜庫 |
| 2 | 樂譜閱讀器 | PDF/圖片顯示、翻頁手勢、工具列 |
| 3 | 註記功能 | PencilKit 手繪、文字註記、儲存/載入 |
| 4 | 自訂區域翻頁 | 區域編輯器、部分翻頁動畫、狀態機 |
| 5 | 曲目清單 & 打磨 | Setlist、Dark mode、Accessibility |

## Key Design Decisions

| 決策 | 原因 |
|------|------|
| SwiftData | 新專案 iOS 17+，原生整合 SwiftUI @Query |
| Snapshot-based partial turns | 避免操作 PDFView 內部 scroll view |
| Normalized 座標 (0.0-1.0) | 區域定義不受裝置尺寸/縮放影響 |
| PencilKit | Apple Pencil 延遲優化、壓力感應 |

## Requirements

- iOS 17.0+
- Xcode 15.0+
- Swift 5.9+

## License

MIT
