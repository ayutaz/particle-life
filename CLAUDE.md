# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

Particle Life Simulation - 原子状の粒子間の引力・斥力ルールを用いて、自己組織化する生命のようなパターンを生成するシミュレーション。C++、JavaScript (2D/3D)、Pythonの3言語で実装されている。

## アーキテクチャ

### C++ 版 (メイン実装: `particle_life/`)
- **openFrameworks** ベースのデスクトップアプリケーション (Windows向け Visual Studio プロジェクト)
- `particle_life/src/ofApp.cpp` - コアアルゴリズム（最初の約100行）とGUI制御。**変更のほとんどはこのファイルに対して行う**
- `particle_life/src/ofApp.h` - `colorGroup` 構造体 (SoA: pos/vel/color) と全GUIスライダーの定義
- `particle_life/src/main.cpp` - エントリーポイント（ウィンドウ設定のみ）
- GUI: `ofxGui` アドオン使用 (`addons.make`)
- 粒子数は64の倍数に制限される（ベクトル化のため）
- 4色固定: green, red, white, yellow。各色ペアに力(power)と半径(radius)パラメータを持つ (4x4 = 16ペア)

### JavaScript 版
- `particle_life.html` - 2D版。Canvas 2D + `lil-gui`。色数・パラメータを動的に設定可能
- `particle_life_3d.html` - 3D版。Three.js + `lil-gui` + OrbitControls

### Python 版
- `particle_life.py` - pygame使用の簡易教育向け実装

### コアアルゴリズム（全実装共通）
各色グループ間で O(n^2) のペアワイズ距離計算を行い、距離が半径内なら `F = g/d` の力を適用。速度に粘性減衰(`viscosity`)をかけて位置更新。壁反発と重力もオプションで適用可能。

## ビルド (C++)

1. [openFrameworks v0.11.2](https://openframeworks.cc/) をダウンロード
2. openFramework の projectGenerator で `particle_life/` フォルダをインポート
3. Visual Studio でビルド (`.sln` ファイルを開く)

コンパイル済みバイナリ: `particle_life/bin/particle_life.exe` (Windows)、`particle_life/pl-compiled.zip`

## JavaScript 版の実行

`particle_life.html` または `particle_life_3d.html` をブラウザで直接開く。

## Python 版の実行

```
pip install pygame
python particle_life.py
```

## 重要な設計上の制約

- 衝突検出を意図的に省略しており、これにより数千粒子のリアルタイムシミュレーションが可能
- C++ 版のセーブ/ロードダイアログは Windows でのみテスト済み
- TBB による並列化コードがコメントアウトで残っている (`ofApp.cpp`)
- `bin/interesting_models/` にプリセットパラメータファイルが保存されている
