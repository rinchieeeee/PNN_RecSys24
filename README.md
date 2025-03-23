# Unlocking the Hidden Treasures: Enhancing Recommendations with Unlabeled Data (RecSys24) の再現実装

## 概要
- 表題の通り、RecSys2024 で発表されていた手法の再現実装を行った. 
- 結果は以下の通り. 論文に記載されている評価指標との差分は、ハイパーパラメータチューニング等で埋められるとみなし、再現実装は成功したとみなすことにした。

MovieLens-1M
| model | Recall@10 | HitRatio@10 | NDCG@10 |
| --- | --- | --- | --- |
|BPR-MF (w/o PNN loss ) | 0.1635 | 0.7419 | 0.2554 | 
|ours: BPR-MF (with PNN loss) | 0.1810 | 0.7717 | 0.2802 | 
|Paper: BPR-MF (with PNN loss) | 0.1840 | 0.7753 | 0.2835 |


FourSquare

| model | Recall@10 | HitRatio@10 | NDCG@10 |
| --- | --- | --- | --- |
|BPR-MF (w/o PNN loss ) | 0.0262 | 0.1717 | 0.0283 | 
|ours: BPR-MF (with PNN loss) | 0.0321 | 0.2013 | 0.0323 | 
|Paper: BPR-MF (with PNN loss) | 0.0332 | 0.2041 | 0.0352 |

## 前提情報
### 実行環境
- OS: Ubuntu 20.04.6 LTS
- GPU: NVIDIA GeForce RTX 3090
    - Driver Version: 535.183.01
- CUDA version: 12.1, (V12.1.105)
- ハードウェア
    - メモリ: 128GB
    - CPU: Intel Core i9-11900 (2.50GHz)

- poetry 2.1.1
- pyenv 2.4.10

※ pyenv
## 実行手順

### python 環境構築

```
cd rinchi-RecBole # rinchi-RecBole へ移動

pyenv local 3.11.4
poetry env use 3.11.4
poetry install --no-root
```

### モデル学習と評価

#### MovieLens-1M

Baseline(BPR-MF)
```
poetry run python run_recbole.py --model=BPR --dataset=ml-1m
```

提案手法(BPR-MF w/ PNN)
```
poetry run python run_recbole.py --model=PNN_RecSys2024 --dataset=ml-1m
```

結果:
| model | Recall@10 | HitRatio@10 | NDCG@10 |
| --- | --- | --- | --- |
|BPR-MF (w/o PNN loss ) | 0.1635 | 0.7419 | 0.2554 | 
|ours: BPR-MF (with PNN loss) | 0.1810 | 0.7717 | 0.2802 | 
|Paper: BPR-MF (with PNN loss) | 0.1840 | 0.7753 | 0.2835 |

#### FourSquare
Baseline(BPR-MF)
```
poetry run python run_recbole.py --model=BPR --dataset=foursquare-nyc-merged --ITEM_ID_FIELD=venue_id --load_col="{'inter': ['user_id', 'venue_id', 'timestamp']}"
```

提案手法(BPR-MF w/ PNN)
```
poetry run python run_recbole.py --model=PNN_RecSys2024 --dataset=foursquare-nyc-merged --config_files=./recbole/properties/model/PNN_RecSys2024_foursquare.yaml
```

結果:
| model | Recall@10 | HitRatio@10 | NDCG@10 |
| --- | --- | --- | --- |
|BPR-MF (w/o PNN loss ) | 0.0262 | 0.1717 | 0.0283 | 
|ours: BPR-MF (with PNN loss) | 0.0321 | 0.2013 | 0.0323 | 
|Paper: BPR-MF (with PNN loss) | 0.0332 | 0.2041 | 0.0352 |