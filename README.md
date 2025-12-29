# 女子高校生対象AI活用型データサイエンス教育：分析コード

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

## 概要

本リポジトリは、以下の論文に関連する分析コードと可視化スクリプトを提供します。

> 小野原彩香 (2025). 女子高校生対象AI活用型データサイエンス教育―文京学院大学女子高等学校セミナー実践報告―. *社会情報教育研究センター研究紀要『社会と統計』*, 12.

## 研究概要

女子高校生を対象とした2日間のデータサイエンスセミナーの実践報告です。生成AIを「学習の伴走者」として活用し、Google Colab上での可視化演習を中心に実施しました。

### 主な分析内容

- **自己効力感の事前・事後比較**: Wilcoxon符号付き順位検定（FDR補正）
- **効果量の算出**: paired Cohen's d
- **事後アンケート分析**: 態度・有用感・高大連携・SDG5関連項目
- **希望進路別の探索的分析**: Kruskal-Wallis検定
- **自由記述の可視化**: ワードクラウド

## ディレクトリ構成

```
.
├── README.md
├── LICENSE
├── requirements.txt
├── data/
│   ├── README.md          # データ構造の説明
│   └── sample_structure.csv   # データ構造のサンプル（ダミー）
├── notebooks/
│   ├── 01_data_preprocessing.ipynb
│   ├── 02_descriptive_analysis.ipynb
│   ├── 03_statistical_tests.ipynb
│   └── 04_visualization.ipynb
├── outputs/
│   └── figures/           # 生成されたグラフ
└── docs/
    └── survey_items.md    # アンケート項目一覧
```

## 環境構築

### 必要なライブラリ

```bash
pip install -r requirements.txt
```

### Google Colabでの実行

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

1. `notebooks/`内のノートブックをGoogle Colabにアップロード
2. データファイルをGoogle Driveにアップロード
3. セルを順番に実行

## 分析の再現

### 1. データの準備

倫理的配慮により、生データは公開していません。データ構造は`data/README.md`を参照してください。

### 2. 分析の実行

```python
# 統計検定の実行例
from scipy import stats
import numpy as np

# Wilcoxon符号付き順位検定
stat, p = stats.wilcoxon(after_scores, before_scores, alternative='greater')

# FDR補正（Benjamini-Hochberg法）
def benjamini_hochberg(p_values):
    n = len(p_values)
    sorted_idx = np.argsort(p_values)
    sorted_p = np.array(p_values)[sorted_idx]
    q_values = sorted_p * n / np.arange(1, n + 1)
    q_values = np.minimum.accumulate(q_values[::-1])[::-1]
    result = np.empty(n)
    result[sorted_idx] = q_values
    return np.clip(result, 0, 1)

# 効果量（paired d）
def cohens_d_paired(before, after):
    diff = after - before
    return diff.mean() / diff.std(ddof=1)
```

## 主な結果

| 項目 | 事前M(SD) | 事後M(SD) | d | q値 |
|------|-----------|-----------|------|------|
| Q1 整理 | 2.34(1.07) | 3.05(1.01) | 0.60 | .004** |
| Q2 読解 | 3.03(1.17) | 3.26(1.11) | 0.21 | .107 |
| Q3 プログラム | 2.50(1.06) | 2.92(0.94) | 0.36 | .013* |
| Q4 課題設定 | 2.79(1.34) | 3.32(0.99) | 0.51 | .004** |

*q<.05, **q<.01（FDR補正後）

## 引用

本リポジトリを使用する場合は、以下の形式で引用してください。

```bibtex
@article{onohara2025ds,
  title={女子高校生対象AI活用型データサイエンス教育―文京学院大学女子高等学校セミナー実践報告―},
  author={小野原彩香},
  journal={社会情報教育研究センター研究紀要『社会と統計』},
  volume={12},
  year={2025},
  publisher={立教大学社会情報教育研究センター}
}
```

## ライセンス

MIT License - 詳細は[LICENSE](LICENSE)を参照してください。

## 連絡先

- **著者**: 小野原彩香
- **所属**: 立教大学 社会情報教育研究センター
- **Email**: [要追加]

## 更新履歴

- 2025-XX-XX: 初版公開
