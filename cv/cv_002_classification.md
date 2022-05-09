# classification

## 主要なモデル一覧

- [LeNet 1989](./cv_002_classification/lenet.md) / [論文](https://direct.mit.edu/neco/article-abstract/1/4/541/5515/Backpropagation-Applied-to-Handwritten-Zip-Code?redirectedFrom=fulltext)
- [AlexNet 2012.09](./cv_002_classification/alexnet.md) / [論文](https://dl.acm.org/doi/abs/10.1145/3065386)
- [VGGNet 2014.09.04 ICLR'2015](./cv_002_classification/vgg.md) / [論文](https://arxiv.org/abs/1409.1556)
- GoogLeNet(Inception-v1) 2014.09.17 / [論文](https://arxiv.org/abs/1409.4842)
- [ResNet 2015.12.10 CVPR'2016](./cv_002_classification/resnet.md) / [論文](https://arxiv.org/abs/1512.03385)
- Inception-v4 2016.02.23 / [論文](https://arxiv.org/abs/1602.07261)
- SqueezeNet 2016.02.24 / [論文](https://arxiv.org/abs/1602.07360)
- [DenseNet 2016.08.25 CVPR'2017](./cv_002_classification/densenet.md) / [論文](https://arxiv.org/abs/1608.06993)
  - ResNetを進化させ、より効率的に深くまで情報を伝搬させるdense blockを構成。
  - dense blockはそのレイヤまでのblock内すべての出力を入力として用いる。
  - また連結方法は加算ではなく、concatenateすることにより情報が消えることを防ぐ。
  - ResNetよりモデル規模を縮小し、性能も改善。

- Xception 2016.01.07 / [論文](https://arxiv.org/abs/1610.02357)
- [ResNeXt 2016.11.16 CVPR'2017](./cv_002_classification/resnext.md) / [論文](https://arxiv.org/abs/1611.05431)
- [MobileNet 2017.04.17](./cv_002_classification/mobilenet.md) / [論文](https://arxiv.org/abs/1704.04861)
- ShuffleNet 2017.07.04 / [論文](https://arxiv.org/abs/1707.01083)
- SENet 2017.09.05 CVPR'2018 / [論文](https://arxiv.org/abs/1709.01507)
- [MobileNetV2 2018.01.13 CVPR'2018](./cv_002_classification/mobilenet_v2.md) / [論文](https://arxiv.org/abs/1801.04381)
- [Big-Little Network 2018.07.10](./cv_002_classification/big_little.md) / [論文](https://arxiv.org/abs/1807.03848)
- ShuffleNetV2 2018.07.30 / [論文](https://arxiv.org/abs/1807.11164)
- Mnasnet 2018.07.31 CVPR'2019 / [論文](https://arxiv.org/abs/1807.11626)
- [Octave Convolution 2019.04.10 ICCV'2019](./cv_002_classification/octave.md) / [論文](https://arxiv.org/abs/1904.05049)
- MobileNetV3 2019.05.06 ICCV'2019 / [論文](https://arxiv.org/abs/1905.02244)
- [EfficientNet 2019.05.28 ICML'2019](./cv_002_classification/efficientnet.md) / [論文](https://arxiv.org/abs/1905.11946)
- [MixNet 2019.07.22 BMVC'2019](./cv_002_classification/mixnet.md) / [論文](https://arxiv.org/abs/1907.09595)
- Noisy Student 2019.11.11 CVPR'2020 / [論文](https://arxiv.org/abs/1911.04252)
- [CSPNet 2019.11.27](./cv_002_classification/cspnet.md) / [論文](https://arxiv.org/abs/1911.11929)
  - DenseNetの更なる高速化。安価なGPUで動作することを念頭に改良。
  - ベースレイヤを2つに分割し、2つのパスで伝搬する情報を効率化している。
  - これは他のネットワークにも容易に適用可能で、計算量を10～20%削減し、さらに精度を向上。

- RegNet 2020.03.30 CVPR'2020 / [論文](https://arxiv.org/abs/2003.13678)
- Vision Transformer 2020.09.29 ICLR'2021 / [論文](https://openreview.net/forum?id=YicbFdNTTy)
- DeiT 2020.12.23 / [論文](https://arxiv.org/abs/2012.12877)
- EfficientNetV2 2021.04.01 ICML'2021 / [論文](https://arxiv.org/abs/2104.00298)
- ConvNeXt 2022.01.10 CVPR'2022 / [論文](https://arxiv.org/abs/2201.03545)

## 参考

- [2019.10.30] 2019年最強の画像認識モデルEfficientNet解説
  - https://qiita.com/omiita/items/83643f78baabfa210ab1

- [2020.09.09] MobileNet(v1,v2,v3)を簡単に解説してみた
  - https://qiita.com/omiita/items/77dadd5a7b16a104df83

- [2021.04.17] EfficientNet B0〜B7で画像分類器を転移学習してみる
  - https://zenn.dev/kleamp1e/articles/202104-efficientnet

- [2021.07.30] EfficientNet: 複合スケールによる効率的な画像分類器
  - https://kikaben.com/efficientnet/

- Neural Network Console
  - https://www.youtube.com/channel/UCRTV5p4JsXV3YTdYpTJECRA

- 6つのモデルでのSwish関数の実験
  - https://ichi.pro/6-tsu-no-moderu-de-no-swish-kansu-no-jikken-265570078399001

- [2019.10.14] 【深層学習】CNNを用いた画像分類手法まとめ（VGG, ResNet, Inceptionなど）
  - https://ys0510.hatenablog.com/entry/cnn_backbone

- ResNetおよびDenseNetの解説
  - https://deepsquare.jp/2020/04/resnet-densenet/

- ResNetからResNextまで
  - https://cvml-expertguide.net/terms/dl/cnn-backbone/resnet/

- 古めな記事だが、ResNetの詳細が記載
  - https://deepage.net/deep_learning/2016/11/30/resnet.html

- 各種モデルサイズの比較表がある。
  - https://keras.io/ja/applications/

- [2021.05.03] 画像認識の大革命。AI界で話題爆発中の「Vision Transformer」を解説！
  - https://qiita.com/omiita/items/0049ade809c4817670d7

- [2020.03.07] 画像認識の最新SoTAモデル「Noisy Student」を徹底解説！
  - https://ai-scholar.tech/articles/treatise/noisy-student-ai-379

- [2021.04.13] 2021年最強になるか！？最新の画像認識モデルEfficientNetV2を解説
  - https://qiita.com/omiita/items/1d96eae2b15e49235110

- [2022.01.13] Transformer(ViT)系より良いConvだけのネットワーク出たよ（画像認識向け）
  - https://qiita.com/TeamN/items/edee1b3803a1d77fc252

- B0～B7の構造
  - https://towardsdatascience.com/complete-architectural-details-of-all-efficientnet-models-5fd5b736142