# 3D Model Viewer

日本語 | [English](README.en.md)

## 概要

ブラウザーだけで動作する、単一HTMLの3Dモデルビューアーです。インストール不要で、対応ブラウザーがあれば Windows・macOS・Linux・iPhone・Android など、OSを問わず利用できます（外部ライブラリの取得のため、オンライン環境が必要です）。

## 主な特徴

- 単一HTML・インストール不要
- OSや環境を問わず、ブラウザーで動作
- モデルとモーションを独立して読み込み可能
- VRM・MMD間のモーション変換
- タッチ操作に対応（スマートフォン・タブレット）
- IndexedDBで履歴・再生リストを保存

> ※ メインの対応形式は VRM / VRMA です。それ以外の形式は近似的な対応となります。

## 対応形式

### 読み込み

| 種類 | 形式 |
|---|---|
| モデル | `.vrm` `.glb` `.pmx` `.pmd` `.zip`（MMD一式） `.fbx` `.obj` |
| モーション | `.vrma` `.vmd` |

### 保存

| 種類 | 形式 |
|---|---|
| モデル | VRM・GLB・MMD |
| モーション | VRMA・VMD |

## 使用している外部モジュール

主に、3D表示・モデル読み込み・VRM処理・モーション変換・ZIP展開のために使用しています。

### 1. 外部モジュール一覧

| モジュール | 主な機能 | このアプリでの使い方 |
|---|---|---|
| Three.js 0.160.0 | 3D描画エンジン | モデル表示、カメラ、照明、ボーン操作、アニメーション再生 |
| three-vrm 3.0.0 | VRM対応 | VRMの読み込み、ヒューマノイドボーン、表情、髪や服の揺れ |
| three-vrm-animation 3.0.0 | VRMA対応 | VRMAの読み込み、VRM用アニメーションへの変換 |
| fflate 0.8.2 | ZIPの圧縮・展開 | ZIPに格納されたMMDモデルやテクスチャの取り出し |
| Ammo.js（Three.js 0.160.0 同梱） | 物理演算 | MMDモデルの剛体・衝突・物理演算（必要なときのみ読み込み） |

### 2. Three.js に付属する拡張モジュール

独立した外部ライブラリではなく、Three.js に同梱されている機能です。

| モジュール | 機能 | 用途 |
|---|---|---|
| GLTFLoader | GLB / glTF の読み込み | 3Dモデルの読み込み |
| MMDLoader | PMX / PMD / VMD の読み込み | MMDモデル・モーションの読み込み |
| FBXLoader | FBX の読み込み | FBXモデルの読み込み |
| OBJLoader | OBJ の読み込み | OBJモデルの読み込み |
| MTLLoader | MTL の読み込み | OBJの材質・テクスチャ設定 |
| MMDAnimationHelper | MMDアニメーションの管理 | VMD再生や物理演算の管理 |
| GLTFExporter | GLB の書き出し | MMD・FBX・OBJなどをGLBに変換 |

補足:

- MMDParser は MMDLoader の内部で使用されます。
- GLTFExporter は、保存操作を行ったときに読み込まれます。

### 3. 実際の使い分け

**モデルの読み込み**

| 形式 | 使用するモジュール |
|---|---|
| VRM | three-vrm |
| GLB | GLTFLoader |
| MMD | MMDLoader ＋ fflate（ZIPの場合） |
| FBX | FBXLoader |
| OBJ | OBJLoader ＋ MTLLoader |

**モーションの読み込み**

- VRMA → three-vrm-animation
- VMD → MMDLoader
- 異なるモデル形式への適用 → アプリ内の独自変換処理

**再生・物理**

- 再生 → Three.js AnimationMixer
- VRMの揺れもの → VRM SpringBone
- MMDの物理 → Ammo.js

**保存**

- GLB → GLTFExporter

### 4. 使用環境・注意点

- 外部モジュールは jsDelivr CDN から取得します。
- インターネット接続が必要です。
- バージョンを固定しているため、ライブラリ更新による突然の仕様変更を抑えられます。
- IndexedDB・localStorage・WebGL などはブラウザー標準の機能であり、外部モジュールではありません。

## まとめ

このアプリの中核は Three.js です。これに、次のものを組み合わせています。

- VRM関連 → three-vrm
- モーション関連 → three-vrm-animation と MMDLoader
- ZIP処理 → fflate
- MMD物理 → Ammo.js

なお、モデル間のモーション変換、接地補正、足IKの近似、指の曲げなどは、HTML内に独自実装しています。

## 権利について

- すべての外部モジュールの権利は、それぞれの制作者に帰属します。
- 読み込むモデル・モーションの権利と利用規約は、それぞれの権利者に従ってください。
- 本アプリは、付属ファイルを含め、改変・再配布は自由です。
- 著作権表示等も不要ですが、入れていただけると喜びます。
