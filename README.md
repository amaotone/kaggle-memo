# kaggle-memo

## 最初に読む

- [bestfittingさんのインタビュー](http://blog.kaggle.com/2018/05/07/profiling-top-kagglers-bestfitting-currently-1-in-the-world/)
- [HF van Veenさんの特徴量作成スライド](https://www.slideshare.net/HJvanVeen/feature-engineering-72376750)
- [nejumiさんのkaggle_memo](https://github.com/nejumi/kaggle_memo)

## EDA

- ヒストグラムにして満足してないか？
- 意味のある順序にソートしてから眺める

## 特徴量作成

- ゼロかそれ以外か、0.5以上か、などの2値学習(tkmさん)

## バリデーション

- KFold or Stratified
  - 多クラス分類ならstratified必須
  - 回帰でも1変数k-meansしてstratifiedにすることも
- Adversarial Validationは検討したか？

## カテゴリ変数の取扱い

- カテゴリの共起を取ってLDAする([Talkingdata 1st place solution](https://www.slideshare.net/TakanoriHayashi3/talkingdata-adtracking-fraud-detection-challenge-1st-place-solution))
- Target-Encodingを考える
- Weight of Evidence ([資料](https://github.com/h2oai/h2o-meetups/blob/master/2017_11_29_Feature_Engineering/Feature%20Engineering.pdf))
- NNのEmbedding Layerに突っ込む

## 欠損の処理

- [Fancy Impute](https://github.com/iskandr/fancyimpute)
- [MIDAS](https://github.com/Oracen/MIDAS)

## Representation Learning

- tSNE
- [UMAP](https://github.com/lmcinnes/umap)
- [Denoising AutoEncoders](https://www.kaggle.com/c/porto-seguro-safe-driver-prediction/discussion/44629#250927)
  - 値をその列の他の値と入れ替えるinputSwapNoiseを加える

## 時系列

## 画像

- imagemagickのidentify --verboseで統計量が出る(tkmさん)

## 自然言語

- 商品カテゴリなどがいくつかのフィールドに分かれているときはすべてくっつけて文章として扱う
- Tfidf+fastTextで強い特徴になるが、fastTextに偏りがちなのでfraction強めにいれると良いらしい ([nerdtreeさん](https://twitter.com/nardtree/status/994579698553311233?s=12))
- fastTextをがっつりPCAなどで次元削減してTfidfと合わせる ([nerdtreeさん](https://twitter.com/nardtree/status/995963496322945025))

## アンサンブル

- 種類の違うモデルは出力のレンジが異なることがあるので、公平に扱う必要がある
- Rankにしたり、何らかの分布に押し込めたあとaverageすると良いことがある
- Stacking
- Quiz Blending ([pdf](https://www.netflixprize.com/assets/GrandPrize2009_BPC_BigChaos.pdf))

## Deep Learning

- [0, 1]のregressionならbinary-crossentropyで学習するのもあり(ynktkさん)
- 連続変数をGaussRankで処理

## LightGBM

- `num_leaves`多め、`feature_fraction`かなり小さめ、とかもあり([Avito 4th](https://www.kaggle.com/c/avito-demand-prediction/discussion/59881))
