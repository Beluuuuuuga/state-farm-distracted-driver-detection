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


| インデックス | ノートブック名 | フォールド | モデル名       | 交差検証法                       | データ拡張           | その他                | Private | Public  | 順位/上位% | 
| ------------ | -------------- | ---------- | -------------- | -------------------------------- | -------------------- | --------------------- | ------- | ------- | ---------- | 
| 1            | Train_001      | 1          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 0.607   | 0.60428 | -          | 
| 2            | Train_001      | 2          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 0.55575 | 0.57041 | -          | 
| 3            | Train_001      | 3          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 0.66393 | 0.65702 | -          | 
| 4            | Train_001      | 4          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 0.50615 | 0.50968 | -          | 
| 5            | Train_001      | 5          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 0.6976  | 0.69935 | -          | 
| 6            | Train_001      | 単純平均   | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 0.33004 | 0.34469 | 220/15.3   | 
| 7            | Train_001      | 加重平均   | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | なし                  | 0.32803 | 0.34204 | 217/15.1   | 
| 8            | Train_001      | 1          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 0.56819 | 0.56271 | -          | 
| 9            | Train_001      | 2          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 0.5372  | 0.51018 | -          | 
| 10           | Train_001      | 3          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 0.5816  | 0.58452 | -          | 
| 11           | Train_001      | 4          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 0.47692 | 0.53186 | -          | 
| 12           | Train_001      | 5          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 0.65748 | 0.62222 | -          | 
| 13           | Train_001      | 単純平均   | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 0.36868 | 0.38685 | 242/16.8   | 
| 14           | Train_001      | 加重平均   | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | 推論時にデータ拡張    | 0.36835 | 0.38487 | 242/16.8   | 
| 15           | Train_002      | 1          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 0.43436 | 0.35424 | -          | 
| 16           | Train_002      | 2          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 0.35106 | 0.442   | -          | 
| 17           | Train_002      | 3          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 0.34256 | 0.32399 | -          | 
| 18           | Train_002      | 4          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 0.39882 | 0.40812 | -          | 
| 19           | Train_002      | 5          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 0.43744 | 0.5564  | -          | 
| 20           | Train_002      | 単純平均   | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 0.24854 | 0.24651 | 134/9.3    | 
| 21           | Train_002      | 加重平均   | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト         | Train_001で半教師学習 | 0.24664 | 0.24554 | 133/9.2    | 
| 22           | Train_003      | 1          | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 0.62581 | 0.58949 | -          | 
| 23           | Train_003      | 2          | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 1.02816 | 1.10323 | -          | 
| 24           | Train_003      | 3          | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 0.98487 | 1.02822 | -          | 
| 25           | Train_003      | 4          | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 0.85652 | 0.81614 | -          | 
| 26           | Train_003      | 5          | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 0.72075 | 0.70645 | -          | 
| 27           | Train_003      | 単純平均   | EfficientNetB0 | ドライバーごとにグループ交差検証 | 回転・シフト         | なし                  | 0.44655 | 0.45771 | -          | 
| 28           | Train_004      | 1          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 0.54219 | 0.55021 | -          | 
| 29           | Train_004      | 2          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 0.56326 | 0.62927 | -          | 
| 30           | Train_004      | 3          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 0.61718 | 0.62281 | -          | 
| 31           | Train_004      | 4          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 0.55999 | 0.61293 | -          | 
| 32           | Train_004      | 5          | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 0.70675 | 0.77808 | -          | 
| 33           | Train_004      | 単純平均   | EfficientNetB0 | クラスごとに層化交差検証         | 回転・シフト・CutMix | なし                  | 0.52447 | 0.56263 | -          | 
| 34           | Train_005      | 1          | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 0.58988 | 0.60445 | -          | 
| 35           | Train_005      | 2          | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 0.60154 | 0.54021 | -          | 
| 36           | Train_005      | 3          | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 0.73495 | 0.72579 | -          | 
| 37           | Train_005      | 4          | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 0.50654 | 0.56818 | -          | 
| 38           | Train_005      | 5          | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 0.77945 | 0.82314 | -          | 
| 39           | Train_005      | 単純平均   | ResNet50       | ランダムに交差検証               | 回転・シフト         | imagenet次元学習あり  | 0.30149 | 0.29799 | 194/13.5   | 
| 40           | Train_006      | 1          | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 0.57427 | 0.50747 | -          | 
| 41           | Train_006      | 2          | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 0.5073  | 0.39326 | -          | 
| 42           | Train_006      | 3          | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 0.47068 | 0.60797 | -          | 
| 43           | Train_006      | 4          | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 0.55838 | 0.44653 | -          | 
| 44           | Train_006      | 5          | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 0.40685 | 0.43112 | -          | 
| 45           | Train_006      | 単純平均   | ResNet50       | ランダムに交差検証               | 回転・シフト         | Train_005で半教師学習 | 0.29437 | 0.27417 | 187/13.0   | 


### ノートブック詳細
**hogehoge**  
ラベルごとに分けられた画像フォルダの状態だと使用しにくいため、csv化し使用しやすい形にした。  

---

**全体**  
各ノートブックでは、フォールド数は5で学習しており、各フォールドで推論を行っている。  
データ拡張手法は回転・水平、垂直シフトを利用し学習している。

---

**Train_001.ipynb**  
モデルはEfficientNetB0、交差検証の方法はラベルごとに一定の分布に保つ層化を利用した。  
データ拡張は回転・水平、垂直シフト・ズームを使用した。特にズームの場合、入賞者のDiscussionでドライバーの周りを物体検出で切り取り精度を出していたため、有効であると考えられる。  
結果は、一番結果の良いフォールドで上位xx%であった。  

また、フォールドでの予測結果を単純平均とパブリックリーダーボードの結果を元にして加重平均でアンサンブルした。  
結果は、単純平均で上位xx%、加重平均で上位xx%であった。 

その後、推論時にデータ拡張（TTA）を行ったときアンサンブル効果でモデルの性能が良くなるため、TTAを行った。  
結果は、各フォールドで全体的に良くなったが、単純平均・加重平均アンサンブルした際に上位xx%であり、アンサンブルでは性能低下が確認された。  
これはTTAする際のデータ拡張手法がアンサンブルに適していなかった可能性があり、性能低下に繋がる方向にアンサンブルされることで特徴量が移動したと思われる。  
今後TTA手法は、アンサンブルにより性能低下が確認されたため、他ノートブックで使用しないことにした。

---

**Train_002.ipynb**  
Train_001で学習された単純平均アンサンブルの結果を利用し、予測の信頼度95%以上の結果を採用して半教師あり学習を行った。  
これは、学習データが2万枚でテストデータが7万枚でテストデータの方が圧倒的に多いため、半教師あり学習を行った。

モデルはEfficientNetB0、交差検証の方法はラベルごとに一定の分布に保つ層化を利用した。  

結果は、単純平均で上位xx%、加重平均で上位xx%でTrain_001より性能向上が確認された。

---

**Train_003.ipynb**  
交差検証の方法をドライバーで分けるグループ化を適用した。  
モデルはEfficientNetB0で学習した。

結果は、単純平均で上位xx%、加重平均で上位xx%でTrain_001より性能低下が確認されたため、
データの分け方でグループ化を採用しないことにした。

---

**Train_004.ipynb**  
データ拡張方法にCutMixを追加した。

結果は、単純平均で上位xx%、加重平均で上位xx%であった。

---

**Train_005.ipynb**  
Train_001と同様だが、モデルをResNet50に変更した。

結果は、単純平均で上位xx%、加重平均で上位xx%であった。

---

**Train_006.ipynb**  
Train_005で学習された単純平均アンサンブルの結果を利用し、予測の信頼度95%以上の結果を採用して半教師あり学習を行った。 

結果は、単純平均で上位xx%、加重平均で上位xx%であった。

---

**最終的な結果**  
Train_002で学習された単純平均のアンサンブルの結果とTrain_006で学習された単純平均のアンサンブルの結果を加重平均でアンサンブルした。  
結果は、上位xx%であった。

## ビルド







