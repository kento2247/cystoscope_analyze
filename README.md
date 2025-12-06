# Friend Network Analysis

6ヶ月間の友人関係ネットワーク変化を分析するためのCytoscapeプロジェクト。

## ファイル構成

| ファイル | 説明 |
|---------|------|
| `friend_network.cys` | Cytoscapeセッションファイル（ネットワークデータ含む） |
| `styles.xml` | 分析用スタイル（7種類） |

## ネットワーク概要

| 指標 | Before（6ヶ月前） | After（6ヶ月後） | 変化 |
|------|------------------|-----------------|------|
| ノード数 | 37 | 41 | +4 |
| エッジ数 | 46 | 67 | +21 (+46%) |
| 平均隣接ノード数 | 2.16 | 2.83 | +0.67 |
| ネットワーク直径 | 4 | 5 | +1 |
| 特性経路長 | 1.93 | 2.27 | +0.34 |
| クラスタ係数 | 0.091 | 0.177 | +0.086 |
| ネットワーク密度 | 0.035 | 0.041 | +0.006 |
| 連結成分数 | 6 | 2 | -4 |

### ノードラベルの読み方

`ID-グループ(性別)` の形式

- 例: `23-1(f)` = ID:23, グループ:1, 女性
- 例: `17-1(m)` = ID:17, グループ:1, 男性

## 使い方

### 1. ネットワークを開く

```
File -> Open -> friend_network.cys
```

### 2. スタイルをインポート

```
File -> Import -> Styles from File -> styles.xml
```

### 3. スタイルを適用

左パネルの **Style** タブ -> ドロップダウンからスタイル名を選択

## 分析用スタイル一覧

| スタイル名 | 説明 | 前提条件 |
|-----------|------|---------|
| `Gender_Color` | 性別で色分け（女性=ピンク、男性=青） | なし |
| `Group_Color` | グループで色分け（1=赤、2=緑、3=青、4=オレンジ） | なし |
| `Gender_Edge_Color` | エッジを性別組み合わせで色分け | なし |
| `Degree_Size` | 入次数でノードサイズ・色が変化 | Analyze Network実行後 |
| `InDegree_Size` | 入次数でノードサイズ・色が変化 | Analyze Network実行後 |
| `Betweenness_Centrality` | 媒介中心性でノードサイズ・色が変化 | Analyze Network実行後 |
| `Comparison_Simple` | Before/After比較用シンプルスタイル | なし |

### Gender_Edge_Color のエッジ色

| 組み合わせ | 色 |
|-----------|-----|
| f->f（女性->女性） | ピンク |
| f->m（女性->男性） | 紫 |
| m->f（男性->女性） | オレンジ |
| m->m（男性->男性） | 青 |

---

## 分析項目一覧と操作手順

### 1. ネットワーク基本統計の算出

ネットワーク全体の統計値を計算する。

**操作手順:**
1. 分析したいネットワーク（before_cs.txt または after_cs.txt）を選択
2. `Tools -> Analyze Network`
3. "Treat network as directed" にチェック（有向グラフとして分析）
4. `OK` をクリック

**算出される指標:**
| 指標 | 説明 |
|-----|------|
| Number of Nodes | ノード数 |
| Number of Edges | エッジ数 |
| Network Diameter | ネットワーク直径（最長経路） |
| Network Radius | ネットワーク半径 |
| Characteristic Path Length | 平均経路長 |
| Avg. Number of Neighbors | 平均隣接ノード数 |
| Network Density | ネットワーク密度 |
| Network Heterogeneity | ネットワーク異質性 |
| Network Centralization | ネットワーク中心化 |
| Clustering Coefficient | クラスタ係数 |
| Connected Components | 連結成分数 |

---

### 2. 入次数中心性（InDegree Centrality）分析

多くの人から友人として選ばれている人（人気者）を特定する。

**操作手順:**
1. `Tools -> Analyze Network` を実行（上記参照）
2. 左パネル Style タブで `Degree_Size` または `InDegree_Size` を選択
3. ノードサイズが大きいほど多くの人から選ばれている

**結果の見方:**
- 大きいノード = 人気が高い中心人物
- Node Tableの `Indegree` 列でソート可能

---

### 3. 出次数（OutDegree）分析

積極的に友人を作っている人を特定する。

**操作手順:**
1. `Tools -> Analyze Network` を実行
2. Node Tableを開く（下パネル）
3. `outdegree` 列をクリックしてソート

**結果の見方:**
- OutDegree高 = 積極的に友人関係を築いている人

---

### 4. 媒介中心性（Betweenness Centrality）分析

異なるグループを繋ぐ橋渡し役を特定する。

**操作手順:**
1. `Tools -> Analyze Network` を実行
2. 左パネル Style タブで `Betweenness_Centrality` を選択
3. ノードが大きく色が濃いほど橋渡し役

**結果の見方:**
- 大きいノード = 情報伝達の要となる人物
- Node Tableの `BetweennessCentrality` 列でソート可能

---

### 5. 近接中心性（Closeness Centrality）分析

ネットワーク全体に最も早くアクセスできる人を特定する。

**操作手順:**
1. `Tools -> Analyze Network` を実行
2. Node Tableを開く（下パネル）
3. `ClosenessCentrality` 列をクリックしてソート

**結果の見方:**
- 値が高い = 他の全員に最短距離でアクセス可能

---

### 6. クラスタ係数（Clustering Coefficient）分析

友人同士が互いに友人かどうか（グループの密度）を測定する。

**操作手順:**
1. `Tools -> Analyze Network` を実行
2. Node Tableを開く（下パネル）
3. `ClusteringCoefficient` 列を確認

**結果の見方:**
- 値が高い = 友人同士も友人（密なグループ内にいる）
- 値が低い = 異なるグループの人々と繋がっている

---

### 7. 性別によるネットワークパターン分析

性別間・性別内の友人関係パターンを可視化する。

**操作手順:**
1. 左パネル Style タブで `Gender_Color` を選択（ノード色分け）
2. または `Gender_Edge_Color` を選択（エッジ色分け）
3. レイアウト適用: `Layout -> yFiles Organic Layout`

**結果の見方:**
- ピンクエッジ（f->f）= 女性同士の友人関係
- 紫エッジ（f->m）= 女性から男性への友人関係
- オレンジエッジ（m->f）= 男性から女性への友人関係
- 青エッジ（m->m）= 男性同士の友人関係

---

### 8. グループ間関係分析

4つのグループ間の関係パターンを分析する。

**操作手順:**
1. 左パネル Style タブで `Group_Color` を選択
2. レイアウト適用: `Layout -> yFiles Organic Layout`
3. 同じ色のノード同士の繋がりを観察

**結果の見方:**
- 赤 = グループ1
- 緑 = グループ2
- 青 = グループ3
- オレンジ = グループ4
- 異なる色間のエッジ = グループ間交流

---

### 9. Before/After 比較分析

6ヶ月間の変化を視覚的に比較する。

**操作手順:**
1. `before_cs.txt` ネットワークを選択
2. `Layout -> yFiles Organic Layout` を適用
3. `Layout -> Copy Node Positions`
4. `after_cs.txt` ネットワークを選択
5. `Layout -> Paste Node Positions`
6. 両方に `Comparison_Simple` スタイルを適用
7. 2つのビューを並べて比較

**結果の見方:**
- 新しいノード = 6ヶ月間に加わった人
- 新しいエッジ = 新しく形成された友人関係
- 消えたエッジ = 途絶えた友人関係

---

### 10. ネットワーク密度の比較

Before/After でネットワークがどれだけ密になったか比較する。

**操作手順:**
1. `before_cs.txt` を選択 -> `Tools -> Analyze Network`
2. Results Panelの `Network Density` を記録
3. `after_cs.txt` を選択 -> `Tools -> Analyze Network`
4. Results Panelの `Network Density` を比較

**結果の見方:**
- 密度増加 = ネットワーク全体が密になった
- 密度減少 = ネットワークが疎になった

---

### 11. エッジ数の変化分析

友人関係の増減を数値で確認する。

**操作手順:**
1. 各ネットワークで `Tools -> Analyze Network`
2. `Number of Edges` を比較
3. Edge Tableで新規エッジを確認

**本データの結果:**
- Before: 46エッジ
- After: 67エッジ
- 変化: +21エッジ（約46%増加）

---

### 12. 連結成分分析

孤立したグループがないか確認する。

**操作手順:**
1. `Tools -> Analyze Network` を実行
2. Results Panelの `Connected Components` を確認
3. 値が1なら全員が繋がっている

**結果の見方:**
- 1 = 全ノードが1つのネットワークに接続
- 2以上 = 孤立したサブグループが存在

---

### 13. 画像エクスポート

分析結果を画像として保存する。

**操作手順:**
1. 表示したいスタイルとレイアウトを適用
2. `File -> Export -> Network to Image...`
3. PNG または PDF を選択
4. 解像度を設定（印刷用は300dpi推奨）
5. `OK` で保存

---

### 14. 統計データのエクスポート

分析結果をCSVで保存する。

**操作手順:**
1. `Tools -> Analyze Network` を実行
2. Node Tableで `File -> Export as CSV...`
3. 必要な列を選択してエクスポート

**エクスポート可能な列:**
- Degree, indegree, outdegree
- BetweennessCentrality
- ClosenessCentrality
- ClusteringCoefficient
- など

---

## クイックリファレンス

| やりたいこと | 操作 |
|-------------|------|
| ネットワーク統計を見る | `Tools -> Analyze Network` |
| 人気者を見つける | Analyze後、`Degree_Size` または `InDegree_Size` スタイル適用 |
| 橋渡し役を見つける | Analyze後、`Betweenness_Centrality` スタイル適用 |
| 性別パターンを見る | `Gender_Edge_Color` スタイル適用 |
| グループ関係を見る | `Group_Color` スタイル適用 |
| 6ヶ月の変化を比較 | 同一レイアウト + `Comparison_Simple` |
| 画像を保存 | `File -> Export -> Network to Image` |
| データをCSV保存 | Node Table -> `File -> Export as CSV` |
