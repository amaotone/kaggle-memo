# kaggle-memo

## 最初に読む

- [bestfittingさんのインタビュー](http://blog.kaggle.com/2018/05/07/profiling-top-kagglers-bestfitting-currently-1-in-the-world/)
- [HF van Veenさんの特徴量作成スライド](https://www.slideshare.net/HJvanVeen/feature-engineering-72376750)
- [nejumiさんのkaggle_memo](https://github.com/nejumi/kaggle_memo)

## EDA

- ヒストグラムにして満足してないか？
- 意味のある順序にソートしてから眺める

## バリデーション

- KFold or Stratified
  - 多クラス分類ならstratified必須
  - 回帰でも1変数k-meansしてstratifiedにすることも
- Adversarial Validationは検討したか？

## カテゴリ変数の取扱い

- カテゴリの共起を取ってLDAする([Talkingdata 1st place solution](https://www.slideshare.net/TakanoriHayashi3/talkingdata-adtracking-fraud-detection-challenge-1st-place-solution))
- Target-Encodingを考える
- Weight of Evidence ([資料](https://github.com/h2oai/h2o-meetups/blob/master/2017_11_29_Feature_Engineering/Feature%20Engineering.pdf))

## 欠損の処理

- [Fancy Impute](https://github.com/iskandr/fancyimpute)
- [MIDAS](https://github.com/Oracen/MIDAS)

## Representation Learning

- tSNE
- [UMAP](https://github.com/lmcinnes/umap)
- [Denoising AutoEncoders](https://www.kaggle.com/c/porto-seguro-safe-driver-prediction/discussion/44629#250927)
  - 値をその列の他の値と入れ替えるinputSwapNoiseを加える

## 時系列

## 自然言語

- Tfidf+fastTextで強い特徴になるが、fastTextに偏りがちなのでfraction強めにいれると良いらしい [[@nerdtreeさんのツイート](https://twitter.com/nardtree/status/994579698553311233?s=12)]
- fastTextをがっつりPCAなどで次元削減してTfidfと合わせる [[@nerdtreeさんのツイート](https://twitter.com/nardtree/status/995963496322945025)]

## アンサンブル

- 種類の違うモデルは出力のレンジが異なることがあるので、公平に扱う必要がある
- Rankにしたり、何らかの分布に押し込めたあとaverageすると良いことがある
- Stacking

## Deep Learning

- 連続変数をGaussRankで処理

