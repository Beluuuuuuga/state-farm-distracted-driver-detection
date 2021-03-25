# state-farm-distracted-driver-detectionコンペティションのレポート
## 概要

概要としては下記の通りです。
- URL：https://www.kaggle.com/c/state-farm-distracted-driver-detection/overview
- 入力：ドライバーの態勢の画像
- 出力：10の状態

```
c0: normal driving
c1: texting - right
c2: talking on the phone - right
c3: texting - left
c4: talking on the phone - left
c5: operating the radio
c6: drinking
c7: reaching behind
c8: hair and makeup
c9: talking to passenger
```

- 評価関数：多クラスのlogloss
- 画像
  - 学習：22,424枚
  - テスト：79,726枚

## リポジトリの構成

```
state-farm-distracted-driver-detection
├─data: csvと画像
│  ├─input: 入力のcsvと画像
│  │  ├─csvs: 入力のcsv
│  │  └─imgs: 入力の画像
│  │      ├─test: テストの画像
│  │      └─train: 学習の画像
│  │          ├─c0
│  │          ├─c1
│  │          ├─c2
│  │          ├─c3
│  │          ├─c4
│  │          ├─c5
│  │          ├─c6
│  │          ├─c7
│  │          ├─c8
│  │          ├─c9
│  │          ├─imgs: 学習の画像
│  │          └─semi-supervised_imgs: 半教師学習のために学習とテストをマージした画像
│  └─output: 提出csv
├─model: モデルの重み
└─notebook: 学習や前処理のノートブック
```

## ノートブックの説明

```
state-farm-distracted-driver-detection
└─notebook
   ├─Preprocessing.ipynb: 学習の際に使用するcsvを作成
   ├─Train_001.ipynb: EfficientNetB0/層化交差検証/データ拡張回転・シフト/アンサンブル/推論時のデータ拡張
   ├─Train_002.ipynb: EfficientNetB0/層化交差検証/データ拡張回転・シフト/アンサンブル/半教師あり学習
   ├─Train_003.ipynb: EfficientNetB0/グループ交差検証/データ拡張回転・シフト/アンサンブル
   ├─Train_004.ipynb: EfficientNetB0/ランダム交差検証/データ拡張回転・シフト・CutMix/アンサンブル
   ├─Train_005.ipynb: ResNet50/層化交差検証/データ拡張回転・シフト/アンサンブル
   └─Train_006.ipynb: ResNet50/層化交差検証/データ拡張回転・シフト/アンサンブル/半教師あり学習

```


## ビルド







