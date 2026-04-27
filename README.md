# Athena Technologies インターン課題

このリポジトリは、Athena Technologies インターン課題の作業環境を管理するためのものです。

## ディレクトリ構成

```text
.
├── README.md
├── README.pdf
├── pyproject.toml
├── uv.lock
├── data/
│   ├── ETTh1.csv
│   ├── ETTh2.csv
│   ├── ETTm1.csv
│   └── ETTm2.csv
└── notebook/
    └── EDA.ipynb
```

## 必要な環境

- Python 3.10 以上
- [uv](https://docs.astral.sh/uv/)

Python の仮想環境と依存関係は `uv` で管理します。

## セットアップ

ロックファイルに基づいて仮想環境を作成・同期します。

```sh
uv sync --locked
```

仮想環境を有効化します。

```sh
source .venv/bin/activate
```


## データ

`data/` ディレクトリには ETT 系列の時系列データセットを配置します。
これらは [ETDataset の ETT-small](https://github.com/zhouhaoyi/ETDataset/blob/main/README.md) に含まれる、2 つの地域・変圧器における電力負荷と油温の 2 年分のデータです。

| ファイル | 元データ名 | 頻度 | ヘッダーを含む行数 | 説明 |
| --- | --- | --- | ---: | --- |
| `ETTh1.csv` | ETT-small-h1 | 1時間ごと | 17,421 | 地域 1 の変圧器データを 1 時間単位に集約したもの |
| `ETTh2.csv` | ETT-small-h2 | 1時間ごと | 17,421 | 地域 2 の変圧器データを 1 時間単位に集約したもの |
| `ETTm1.csv` | ETT-small-m1 | 15分ごと | 69,681 | 地域 1 の変圧器データを 15 分単位で記録したもの |
| `ETTm2.csv` | ETT-small-m2 | 15分ごと | 69,681 | 地域 2 の変圧器データを 15 分単位で記録したもの |

各 CSV は以下の列を持ちます。

```text
date, HUFL, HULL, MUFL, MULL, LUFL, LULL, OT
```

列の意味は以下の通りです。

| 列 | 説明 |
| --- | --- |
| `date` | 記録日時 |
| `HUFL` | 高負荷系統の有効負荷 |
| `HULL` | 高負荷系統の無効負荷 |
| `MUFL` | 中負荷系統の有効負荷 |
| `MULL` | 中負荷系統の無効負荷 |
| `LUFL` | 低負荷系統の有効負荷 |
| `LULL` | 低負荷系統の無効負荷 |
| `OT` | 油温。予測対象の目的変数 |

`data/` は Git 管理対象外です。環境を再現する場合は、必要な CSV ファイルを `data/` 配下に配置してください。

## ノートブック

探索的データ分析は以下のノートブックで行います。

```text
notebook/EDA.ipynb
```

仮想環境を有効化したうえで、Jupyter Notebook や VS Code など任意の Jupyter 対応ツールから開いてください。

## 依存関係の管理

依存パッケージを追加する場合は、次のコマンドを使います。

```sh
uv add <package-name>
```

依存関係を変更した場合は、以下の 2 ファイルを合わせて更新・コミットします。

```text
pyproject.toml
uv.lock
```

これにより、別の環境でも同じ Python 環境を再現できます。
