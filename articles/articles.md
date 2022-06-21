# 記事のスクラップ

## 一般

- 敵対的学習を使う場合、活性化関数を修正した方が良いかもしれないという話
  - https://ai-scholar.tech/articles/adversarial-perturbation/smooth-adversarial-training
* Poincare Embeddings
  * [異空間への埋め込み！Poincare Embeddingsが拓く表現学習の新展開 - ABEJA Tech Blog](https://tech-blog.abeja.asia/entry/poincare-embeddings)
  * 双曲空間に埋め込めば非常に低次元で表現できる埋め込みベクトル。

## バイオ系

- PyMOLが使われることが多いようだ。
  - https://hira-labo.com/archives/209
  - https://hira-labo.com/archives/1882
  - https://hira-labo.com/archives/1544

## Kaggle

- KaggleのH&Mのレコメンド、良コンペだったという噂が聞こえてきます。
  - 候補抽出⇒並べ替えの2stageレコメンドが上位ランク
  - 特徴量の作りこみが大事で、NNは微妙だったようだ
  - https://yng87.github.io/blog/2022/05/kaggle_hm/
  - https://zenn.dev/zerebom/articles/9e6bad764d3f97

## 時系列データ

- SREのためのシステム障害における異常検知
  - https://speakerdeck.com/yuukit/sre-next-2022

## ライブラリ

- cuml
  - sklearnをGPUで高速にしたnvidiaのライブラリ
  - https://github.com/rapidsai/cuml

## CV

- Random Erasing
  - https://qiita.com/takurooo/items/a3cba475a3db2c7272fe

- mixup
  - 新しめの例（しかし単体の実装例
    - https://tensorflow.classcat.com/2021/12/01/keras-2-examples-vision-mixup/
  - そもそもImageDataGeneratorとは
    - https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator
  - ImageDataGeneratorを拡張実装例あり（参考になるものの、いくつか疑問のある実装...）
    - https://qiita.com/koshian2/items/909360f50e3dd5922f32
  - ImageDataGeneratorを拡張した実装例２（こっちの方が今見ると筋がいい実装）
    - https://maxigundan.com/deeplearning/2020/04/%E3%80%90tf2%E3%80%91imagedatagenerator%[…]%9Caugmentation%E6%A9%9F%E8%83%BD%E3%82%92%E8%BF%BD%E5%8A%A0/
  - mixup単体の実装例
    - https://qiita.com/yu4u/items/70aa007346ec73b7ff05
  - keras公式のmixup。one_hotの部分は参考にした。
    - https://keras.io/examples/vision/mixup/
  - mixupの理論
    - https://wazalabo.com/mixup_1.html

- ShakeDrop
  - https://qiita.com/yu4u/items/a9fc529c85534eca11e5
  - パラメータの更新方法に関するもの。