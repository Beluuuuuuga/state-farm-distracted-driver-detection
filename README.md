# state-farm-distracted-driver-detectionコンペティションのレポート
## 概要

概要は下記の通り。
- URL：https://www.kaggle.com/c/state-farm-distracted-driver-detection/overview
- 入力：ドライバーの画像
- 出力：10のドライバーの状態

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
- 用意されている画像
  - 学習：22,424枚
  - テスト：79,726枚
- 用意されているCSV
  - driver_imgs_list.csv：ドライバーと画像を紐づけているCSV 
  - sample_submission.csv：サンプル提出のためのCSV

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

## アプローチと結果
### データ探索と方針

driver_imgs_list.csvでユニークなドライバーを確認すると26人であり  
ドライバーを元にした画像の分け方が重要であると判断した。

また、学習とテストの画像数が大きく異なり、テストデータが学習データの3倍存在しており  
テストデータを学習にどのように使用するかもモデルの性能に大きく影響してくると考えられた。

大きな方針は下記の通りである。
- グループ化・層化などデータの分け方に注意する
- 半教師あり学習でテストデータをモデルに学習させる
- 交差検証で作成されたモデルでアンサンブル
- 複数のモデルで学習しアンサンブル
- 学習時はFold数は5に設定し学習



### ノートブック概要

各ノートブックで使用したモデルや手法は下記の通り。

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

### 結果一覧


| ノートブック名 | フォールド   | モデル名       | 交差検証法                       | データ拡張           | その他                | Public | Private | 上位% | 
| -------------- | ------------ | -------------- | -------------------------------- | -------------------- | --------------------- | ------ | ------- | ----- | 
| Train_001      | 1            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_001      | 2            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_001      | 3            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_001      | 4            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_001      | 5            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_001      | アンサンブル | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_001      | 1            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 1      | 2       | 3     | 
| Train_001      | 2            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 1      | 2       | 3     | 
| Train_001      | 3            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 1      | 2       | 3     | 
| Train_001      | 4            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 1      | 2       | 3     | 
| Train_001      | 5            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 1      | 2       | 3     | 
| Train_001      | アンサンブル | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 1      | 2       | 3     | 
| Train_002      | 1            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 1      | 2       | 3     | 
| Train_002      | 2            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 1      | 2       | 3     | 
| Train_002      | 3            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 1      | 2       | 3     | 
| Train_002      | 4            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 1      | 2       | 3     | 
| Train_002      | 5            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 1      | 2       | 3     | 
| Train_002      | アンサンブル | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 1      | 2       | 3     | 
| Train_003      | 1            | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_003      | 2            | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_003      | 3            | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_003      | 4            | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_003      | 5            | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_003      | アンサンブル | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 1      | 2       | 3     | 
| Train_004      | 1            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 1      | 2       | 3     | 
| Train_004      | 2            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 1      | 2       | 3     | 
| Train_004      | 3            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 1      | 2       | 3     | 
| Train_004      | 4            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 1      | 2       | 3     | 
| Train_004      | 5            | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 1      | 2       | 3     | 
| Train_004      | アンサンブル | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 1      | 2       | 3     | 
| Train_005      | 1            | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 1      | 2       | 3     | 
| Train_005      | 2            | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 1      | 2       | 3     | 
| Train_005      | 3            | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 1      | 2       | 3     | 
| Train_005      | 4            | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 1      | 2       | 3     | 
| Train_005      | 5            | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 1      | 2       | 3     | 
| Train_005      | アンサンブル | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 1      | 2       | 3     | 
| Train_006      | 1            | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 1      | 2       | 3     | 
| Train_006      | 2            | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 1      | 2       | 3     | 
| Train_006      | 3            | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 1      | 2       | 3     | 
| Train_006      | 4            | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 1      | 2       | 3     | 
| Train_006      | 5            | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 1      | 2       | 3     | 
| Train_006      | アンサンブル | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 1      | 2       | 3     | 


### ノートブック詳細


## ビルド







