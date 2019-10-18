# Kaggle Memo

個人的に学んだことをメモしておく場所です。Kaggleを始めたいという方は[Getting Started](https://github.com/amaotone/kaggle-memo/blob/master/getting_started.md)も参考にしてみてください。

## 多くの人に役立ちそうなリンク

- [bestfittingさんのインタビュー](http://blog.kaggle.com/2018/05/07/profiling-top-kagglers-bestfitting-currently-1-in-the-world/)
- [HF van Veenさんの特徴量作成スライド](https://www.slideshare.net/HJvanVeen/feature-engineering-72376750)
- [nejumiさんのkaggle_memo](https://github.com/nejumi/kaggle_memo)

## EDA

- 時系列性がある場合などは特に、意味のある順序にソートしてから眺める
- ランダムにサンプリングしてcsvに吐き、Excelで眺める
- 各列の意味を和訳しながらデータを眺める。その過程でいろいろ気づくことがあるので、メモしておく

## バリデーション

- すぐれたバリデーションを作ることがコンペの第一歩
  - 今後の全てのアイディアの評価はバリデーションの精度で行う。悪いバリデーションに最適化するのは狂った方位磁針に沿って航海するのと一緒。
- KFold or Stratified
  - 多クラス分類ならstratified必須
  - 回帰でも1変数k-meansしてstratifiedにすることも
- Adversarial Validationは検討したか？
- random seedを変えてどれくらいばらつくかで自分のバリデーションの精度を確認する(Jackさん)

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
  - 必要ならTarget-Encodingのスムージングも検討する([参考](https://mikebird28.hatenablog.jp/entry/2018/06/14/172132))
- Weight of Evidence ([参考](https://github.com/h2oai/h2o-meetups/blob/master/2017_11_29_Feature_Engineering/Feature%20Engineering.pdf))
- NNのEmbedding Layerに突っ込む
  - cross validationしているとき、各foldでembeddingの実際の値が異なることには注意が必要。実際の値を特徴量にしてLightGBMにかける、とかは難しいかも。
- カテゴリの表記ゆれなどがある場合はn-gramを取るなどして表記揺れをなくすとよい

## 欠損の処理

- [Fancy Impute](https://github.com/iskandr/fancyimpute)
- [MIDAS](https://github.com/Oracen/MIDAS)

## 特徴量選択

- Ridgeなどにかけて1変数ずつ有効かどうか確認するstepwiseを行う。LightGBMなどでできたらそちらのほうが良いが、計算時間との相談。

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

- 時系列的依存性がある場合はバリデーションの切り方に注意
- 基本的には直近のもので学習させたほうが良いが、1年前などに特徴的な周期性がないかは確認すべき
- 時系列で重み付けしたカウントなどは有効な特徴量になり得る
- 過去のflagのn-gramを取る(Jackさん)
- 過去のflag推移を文字列にしてカテゴリ変数として扱うと、複雑なパターンを表現可能になることがある(Jackさん)

### 画像

- imagemagickのidentify --verboseで統計量が出る (tkmさん)
- 画像の動きが欲しい場合はOpenCVのoptical flowを取る (osciiartさん)

### 自然言語処理

#### 前処理

- BeautifulSoupでHTMLタグを消す
- unicodedataライブラリでアクセントを消す
- 省略形を元に戻す(don't -> do notなど)
- シンボルなどを消す
- stemmingやlemmatizationを行う
- stopwordを消す(消すなという意見もあるのでよくわからん)

#### 特徴量

- textblobライブラリでpolarity(ネガポジ)やsubjectivity(主観性)判定が簡単にできる([src](https://medium.com/@rahulvaish/textblob-and-sentiment-analysis-python-a687e9fabe96))
- 商品カテゴリなどがいくつかのフィールドに分かれているときはすべてくっつけて文章として扱う
- Tfidf+fastTextで強い特徴になるが、fastTextに偏りがちなのでfraction強めにいれると良いらしい ([nardtreeさん](https://twitter.com/nardtree/status/994579698553311233?s=12))
- fastTextをがっつりPCAなどで次元削減してTfidfと合わせる ([nardtreeさん](https://twitter.com/nardtree/status/995963496322945025))
- 翻訳→再翻訳してTTAするのは有効 (toxic commentで多く使われた)
- 文章ユニークの単語の割合やすべて大文字の割合をモデルの後半でconcatする(Toxic Comment 3rd)

#### Pretrained Embeddingについて

- pretrained embeddingは横に結合したり、平均をとったりして用いる
- pretrained embeddingを使う場合、語彙ができるだけ多くpretrained embeddingに含まれていることが大事。大文字にしたり小文字にしたり、stemmingを行ったりして、できるだけたくさんの単語に対して学習済みベクトルを割り当てるのが大切([Quora 3rd](https://www.kaggle.com/wowfattie/3rd-place))
- スペル訂正をするときは編集距離1の単語を探すと良い
- Projection Meta Embedding(PME)を検討する
  - 各学習済みの重みをconcatし、Dense+ReLUで低次元に写像してEmbeddingのように使う。

#### その他

- [Googleが指定する有害単語リスト](https://github.com/RobertJGabriel/Google-profanity-words/blob/master/list.txt)

## 手法ごと

### Deep Learning

- [0, 1]のregressionならbinary-crossentropyで学習するのもあり (ynktkさん)
- 連続変数をGaussRankで処理
- 徐々に`batch_size`を大きくしていく ([Mercari 1st](https://www.kaggle.com/c/mercari-price-suggestion-challenge/discussion/50256))
- AdamWとamsgradを使う([参考](http://www.fast.ai/2018/07/02/adam-weight-decay/))
- Kernel Onlyコンペなど、実行時間制限があって多くのモデルのアンサンブルを行えない状況ではSnapshot Ensembleを検討する。
- 自然言語処理において、各バッチごとに系列長を決めることで学習の効率化が可能
  - 系列長の設定はバッチごとの最長でもいいが、95%点などを利用するのも良い(Quora 1st)

### LightGBM

- `num_leaves`多め、`feature_fraction`かなり小さめ、とかもあり ([Avito 4th](https://www.kaggle.com/c/avito-demand-prediction/discussion/59881))
- `feature_fraction = sqrt(n_features)/n_features` 程度だと特徴量数の影響を受けづらく、良い

## Kernel Only / 2-stage制 について

- 当たり前だが、Kernel Onlyコンペだからといって全ての作業をKernelでやらなければいけないわけではない。パラメータチューニングなどはローカルでやり、良いものをKernelに載せる
- 2nd-stageに進むことが一番大切。Kernelがメモリ不足で落ちたり、実行時間制限にかかったりしないように心がける
  - 学習を高速化するテクニックを使うのはもちろんのこと、余計なデータの書き出しなどで時間を使わないこと

## チームで動くときのノウハウ

- slackでチームを作り、特徴量・モデル・メモ・精度などを簡単にシェアしておけるようにする
- 今日使うサブミット数を毎朝slack botなどで聞いて調整する

## おまけ

- https://notify-bot.line.me/my/ からLINEに通知を送るアクセストークンを作れる
  - 実際に通知を送るコードは https://gist.github.com/amaotone/2eea52379d559a4dc60e6c6c76085b7b
- (銅メダル圏内くらいのとき)終了直前で激強kernelが出現することがあるので、最終日は2サブミット残して7時に起床し、自分のsolutionとちゃちゃっとアンサンブルできる体制を整えておく。1サブ残しだと精神的にきついので、2サブ推奨
- 2値分類のコンペではkernel-bestのどこかの行を0にしたやつと1にしたやつを提出すると、カーネルパクリ軍団を抜かせることがある

## ライブラリ

|ライブラリ名|内容|
|:--|:--|
|[eli5](https://eli5.readthedocs.io/en/latest/tutorials/index.html)|デバッグと可視化。permutation importanceとかLIMEとか|
|[PDPBox](https://pdpbox.readthedocs.io/en/latest/)|partial dependency plotの作成|
|[albumentation](https://github.com/albu/albumentations)|画像のaugmentation|


## リンク集

### 参戦記

- [Quora Insincere Questions Classification](https://amalog.hateblo.jp/entry/quora-insincere-questions-classification)
- [Home Credit Default Risk](https://amalog.hateblo.jp/entry/kaggle-home-credit)

### Kaggle Meetup

- [Kaggle Tokyo Meetup #5 の資料](https://connpass.com/event/105298/presentation/)
- [Kaggle Tokyo Meetup #4 の資料](https://connpass.com/event/82458/presentation/)
- [Kaggle Tokyo Meetup #3](https://connpass.com/event/66431/)

## 書いている人

- Amane Suzuki
- twitter: [@SakuEji](https://twitter.com/SakuEji)
- kaggle: [@amaotone](https://www.kaggle.com/amaotone)
