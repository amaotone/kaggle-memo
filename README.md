# kaggle-memo

## 最初に読む

- [bestfittingさんのインタビュー](http://blog.kaggle.com/2018/05/07/profiling-top-kagglers-bestfitting-currently-1-in-the-world/)
- [HF van Veenさんの特徴量作成スライド](https://www.slideshare.net/HJvanVeen/feature-engineering-72376750)
- [nejumiさんのkaggle_memo](https://github.com/nejumi/kaggle_memo)

## メタ的な話

[bestfittingさんのインタビュー](http://blog.kaggle.com/2018/05/07/profiling-top-kagglers-bestfitting-currently-1-in-the-world/)より

- 過去問・論文のサーベイをする
- 本格的に取り組むときは良いバリデーションを作る(かなり重要)
- 予測値の分布を見たり間違ったやつの傾向を確認したりして、その知見を元に新しいモデルを作る

## 時系列

## 自然言語

- Tfidf+fastTextで強い特徴になるが、fastTextに偏りがちなのでfraction強めにいれると良いらしい [[@nerdtreeさんのツイート](https://twitter.com/nardtree/status/994579698553311233?s=12)]
- fastTextをがっつりPCAなどで次元削減してTfidfと合わせる [[@nerdtreeさんのツイート](https://twitter.com/nardtree/status/995963496322945025)]
