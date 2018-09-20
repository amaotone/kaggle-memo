# kaggle-memo

まなびながら書き足していきます。

## 最初に読む

- [bestfittingさんのインタビュー](http://blog.kaggle.com/2018/05/07/profiling-top-kagglers-bestfitting-currently-1-in-the-world/)
- [HF van Veenさんの特徴量作成スライド](https://www.slideshare.net/HJvanVeen/feature-engineering-72376750)
- [nejumiさんのkaggle_memo](https://github.com/nejumi/kaggle_memo)

## EDA

- ヒストグラムにして満足してないか？
- 意味のある順序にソートしてから眺める

## バリデーション

- すぐれたバリデーションを作ることがコンペの第一歩
- KFold or Stratified
  - 多クラス分類ならstratified必須
  - 回帰でも1変数k-meansしてstratifiedにすることも
- Adversarial Validationは検討したか？

## 特徴量作成

- ゼロかそれ以外か、0.5以上か、などの2値学習(tkmさん)
- 予測値を特徴量に入れてもう一度学習する(Home Credit)
- メインcsvの1行に対し、サブcsvの複数行が対応しているような場合(Home Creditとか)
  - サブに時系列性があればカテゴリ変数は最後の値を取る、などが使える
  - 逆にサブにメインをマージして、サブだけで学習してみるのも手
  - 先頭/末尾のn行で集約
- 代表的な特徴量だけで計算したkNNで近傍500点のtargetの平均を特徴量として加える(Home Credit 1st)
  - 全変数でやるとnoisyだし、kNNは次元が増えてくると意味をなさなくなってくる
  
  

## カテゴリ変数の取扱い

- カテゴリの共起を取ってLDAする([Talkingdata 1st](https://www.slideshare.net/TakanoriHayashi3/talkingdata-adtracking-fraud-detection-challenge-1st-place-solution))
- Target-Encodingを考える
- Weight of Evidence ([資料](https://github.com/h2oai/h2o-meetups/blob/master/2017_11_29_Feature_Engineering/Feature%20Engineering.pdf))
- NNのEmbedding Layerに突っ込む
  - cross validationしているとき、各foldでembeddingの実際の値が異なることには注意が必要。実際の値を特徴量にしてLightGBMにかける、とかは難しいかも。
- カテゴリの表記ゆれなどがある場合はn-gramを取るなどして表記揺れをなくすとよい

## 欠損の処理

- [Fancy Impute](https://github.com/iskandr/fancyimpute)
- [MIDAS](https://github.com/Oracen/MIDAS)

## Representation Learning

- tSNE
- [UMAP](https://github.com/lmcinnes/umap)
- [Denoising AutoEncoders](https://www.kaggle.com/c/porto-seguro-safe-driver-prediction/discussion/44629#250927)
  - 値をその列の他の値と入れ替えるinputSwapNoiseを加える

## アンサンブル

- 種類の違うモデルは出力のレンジが異なることがあるので、公平に扱う必要がある
- Rankにしたり、何らかの分布に押し込めたあとaverageすると良いことがある
- Stacking
- Quiz Blending ([pdf](https://www.netflixprize.com/assets/GrandPrize2009_BPC_BigChaos.pdf))
- 特徴量をbaggingしたモデルを大量につくってアンサンブル(Home Credit)

## タスクの種類ごと

### 時系列

### 画像

- imagemagickのidentify --verboseで統計量が出る (tkmさん)
- 画像の動きが欲しい場合はOpenCVのoptical flowを取る (osciiartさん)

### 自然言語

- 商品カテゴリなどがいくつかのフィールドに分かれているときはすべてくっつけて文章として扱う
- Tfidf+fastTextで強い特徴になるが、fastTextに偏りがちなのでfraction強めにいれると良いらしい ([nerdtreeさん](https://twitter.com/nardtree/status/994579698553311233?s=12))
- fastTextをがっつりPCAなどで次元削減してTfidfと合わせる ([nerdtreeさん](https://twitter.com/nardtree/status/995963496322945025))

## 手法ごと

### Deep Learning

- [0, 1]のregressionならbinary-crossentropyで学習するのもあり (ynktkさん)
- 連続変数をGaussRankで処理
- 徐々に`batch_size`を大きくしていく ([Mercari 1st](https://www.kaggle.com/c/mercari-price-suggestion-challenge/discussion/50256))

### LightGBM

- `num_leaves`多め、`feature_fraction`かなり小さめ、とかもあり ([Avito 4th](https://www.kaggle.com/c/avito-demand-prediction/discussion/59881))
- `feature_fraction = sqrt(n_features)/n_features` 程度だと特徴量数の影響を受けづらく、良い

## チームで動くときのノウハウ

- slackでチームを作り、特徴量・モデル・メモ・精度などを簡単にシェアしておけるようにする
- 今日使うサブミット数を毎朝slack botなどで聞いて調整する

## おまけ

- (銅メダル圏内くらいのとき)終了直前で激強kernelが出現することがあるので、最終日は2サブミット残して7時に起床し、自分のsolutionとちゃちゃっとアンサンブルできる体制を整えておく。1サブ残しだと精神的にきついので、2サブ推奨
- 2値分類のコンペではkernel-bestのどこかの行を0にしたやつと1にしたやつを提出すると、カーネルパクリ軍団を抜かせることがある

## ライブラリ

|ライブラリ名|内容|
|:--|:--|
|[eli5](https://eli5.readthedocs.io/en/latest/tutorials/index.html)|デバッグと可視化。permutation importanceとかLIMEとか|
|[PDBBox](https://pdpbox.readthedocs.io/en/latest/)|partial dependency plotの作成|

## 参戦記

- [Home Credit Default Risk](https://amalog.hateblo.jp/entry/kaggle-home-credit)
