# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),  
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.1.3] - 2025-11-22

### Changed
- 移除對 `numpy <2` 的不必要限制（允許使用 `numpy>=1.23`）。
- 字型 Fallback 列表優化，提高跨平台穩定性（Windows / macOS / Linux / Colab）。

---

## [0.1.2] - 2025-11-21

### Added
- 支援 Google Colab 自動下載與註冊中文字體（Noto Sans CJK）。
- 新增 `detect_font()` 自動偵測中文字體函式。
- KBar 內建中文顯示設定，無需手動調整 matplotlib 字型參數。

### Added
- 支援 Ubuntu / Raspberry Pi / Linux 環境自動偵測與安裝中文字型（Noto Sans CJK）。
- 自動安裝過程會根據系統顯示字型名稱（如 `Noto Sans CJK JP`）。
- 中文 K 線標題與字體 fallback 顯示更穩定。

### Fixed
- 修正無中文字型時，mplfinance `font.family` 傳入 `None` 造成的警告。
- 修正 Linux 環境 fallback 時可能無法顯示標題的問題。

---
