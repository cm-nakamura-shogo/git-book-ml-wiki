# 物体検出

## 概要

- 物体検出とはclassificationに加えて、localizationが必要なタスクである。
- 物体を囲む矩形領域をbounding boxと呼ぶ。
- 物体検出は以下を予測値として学習する。
  - classification
  - 物体の中心座標x,y(連続値)
  - bounding boxのheightとwidth(連続値)
- 学習の際、正解がない場所についてはback propagationをしないなど工夫をして学習が必要。

- 上記の基本的な手法のままでは、以下の課題がある
  - Convの最終層の画像サイズに依存した個数しか検出できない。(最終層が8x8ならば64個が上限)
  - 中心が同じ領域にあるものを検出できない。(学習時の正解も一つしか設定できない)

- 上記を解決するために、anchor boxを準備する。
  - anchor boxはいろんなサイズ、縦横比のbounding box候補のようなイメージ。
  - anchor boxの種類毎に、上記の基本的な手法で学習を実施する。
  - anchor boxの種類はクラスタリングなどにより求める。
  - anchor boxの種類毎に、最終層の出力解像度を分けるアプローチのモデルもある。

- NMS(Non-maximum Supressions)で候補を削減。
  - anchor boxを用いると、重なりの大きな物体が複数検出されるので、IoUが一定以上の場合は、その中で一番大きいスコアの領域のみを採用する。

- これらの手順は煩雑なので、anchor boxを使わない方法も近年人気である。
  - 最終層を元画像の1/4程度に収めて、高解像で出力するCenterNetなど。

## 学会

- CVPR
- ICCV
- NeurlPS

## レポジトリ(フレームワーク)

- [mmdetection](https://github.com/open-mmlab/mmdetection)
  - OpenMMLabによるレポジトリ

- [PaddleDetection](https://github.com/PaddlePaddle/PaddleDetection)
  - Baidu社によるPaddlePaddleというフレームワークを用いたレポジトリ

- [detectron2](https://github.com/facebookresearch/detectron2)
  - FAIRによるレポジトリ

- まとめ
  - [https://aru47.hatenablog.com/entry/2022/01/01/123929](https://aru47.hatenablog.com/entry/2022/01/01/123929)

## 主要なモデル一覧

- [HOG+SVM 2005/06/20](./cv_003_object_detection/hog_svm.md) / [論文](http://lear.inrialpes.fr/people/triggs/pubs/Dalal-cvpr05.pdf)
  - CNN誕生前のモデルで良く参照された論文
  - HOGはpixel変化を捉える特徴量
  - HOG特徴量を用いてSVMで多クラス分類する
  - NMSはこの時から使われている

- [R-CNN 2013/11/11](./cv_003_object_detection/r_cnn.md), 2stage / [論文](https://arxiv.org/abs/1311.2524) / [実装例](https://paperswithcode.com/paper/rich-feature-hierarchies-for-accurate-object)
  - CNN適用の先駆け論文
  - selective searchという古典的手法で領域候補を抽出
  - 候補領域をリサイズしてCNNに入力して特徴量ベクトルを得る
  - その後段で1-class SVMとbounding boxのregressionを実施

- [SPP-net 2014/06/18](./cv_003_object_detection/spp_net.md), 2stage / [論文](https://arxiv.org/abs/1406.4729) / [実装例](https://paperswithcode.com/paper/spatial-pyramid-pooling-in-deep-convolutional)
  - 領域候補の切り出しを特徴量マップに対して実施し効率化
  - これにより最大2000毎程度ある領域候補のCNN処理が1回で実現可能
  - 切り出した特徴量マップはSPPで固定長の特徴量に変換して後段で処理

- [Fast R-CNN 2015/04/30](./cv_003_object_detection/fast_r_cnn.md), 2stage / [論文](https://arxiv.org/abs/1504.08083) / [実装例](https://paperswithcode.com/paper/fast-r-cnn)
  - Multi-task lossによりclassificationとbounding box推定を同時学習
  - 固定長の特徴量変換としてSPPの代わりにRoI Poolingを使用
  - RoI Poolingは両機をグリッド分割し各グリッドに対してpooling処理を実施

- [Faster R-CNN 2015/06/04](./cv_003_object_detection/faster_r_cnn.md), 2stage / [論文](https://arxiv.org/abs/1506.01497) / [実装例](https://paperswithcode.com/paper/faster-r-cnn-towards-real-time-object)
  - 領域候補を抽出するselective searchをCNNで置き換え
  - このNetworkをRPN(Region Proposal Network)と呼ぶ
  - RPNには9個のanchor boxを使用し領域有無とbounding box回帰を実施

- [YOLO 2015/06/08](./cv_003_object_detection/yolo.md), 1stage (anchor free) / [論文](https://arxiv.org/abs/1506.02640) / [実装例](https://paperswithcode.com/paper/you-only-look-once-unified-real-time-object)
  - 領域検出とclassificationの同時学習を実現(You only look once)
  - 入力画像をグリッド分割し、各グリッドに固定数のbounding box回帰を実施

- [SSD 2015/12/08](./cv_003_object_detection/ssd.md), 1stage / [論文](https://arxiv.org/abs/1512.02325) / [実装例](https://paperswithcode.com/paper/ssd-single-shot-multibox-detector)
  - YOLOと同じく領域検出とclassificationの同時学習を実現(Single shot detector)
  - backboneの途中層の高解像な特徴量マップを使用
  - 特徴量マップの解像度毎に様々な大きさのanchor boxを定義

- [R-FCN 2016/05/20](./cv_003_object_detection/r_fcn.md), 2stage / [論文](https://arxiv.org/abs/1605.06409) / [実装例](https://paperswithcode.com/paper/r-fcn-object-detection-via-region-based-fully)
  - RoI Pooling以降の全結合層をCNN化したモデル
  - RoI Poolingの前段にposition sensitive mapという畳み込みを追加
  - その後段でRoI Poolingを実施する構成

- [FPN 2016/12/09](./cv_003_object_detection/fpn.md), 2stage / [論文](https://arxiv.org/abs/1612.03144) / [実装例](https://paperswithcode.com/paper/feature-pyramid-networks-for-object-detection)
  - ピラミッド構造の特徴量マップを実現したモデル
  - ベースはFaster R-CNNであり、RPNとclassificationどちらもピラミッド構造化
  - FPNの考え方は現在まで採用されており重要な論文

- [YOLOv2(YOLO9000) 2016/12/25](./cv_003_object_detection/yolo_v2.md), 1stage / [論文](https://arxiv.org/abs/1612.08242) / [実装例](https://paperswithcode.com/paper/yolo9000-better-faster-stronger)
  - YOLOをベースに改良を実施
  - 入力の高解像化、anchor boxの導入、高解像な特徴量マップの使用など
  - それ以外に9000クラスまで分類可能なWordTreeというアーキテクチャを構築

- [RetinaNet 2017/08/07](./cv_003_object_detection/retinanet.md), 1stage / [論文](https://arxiv.org/abs/1708.02002) / [実装例](https://paperswithcode.com/paper/focal-loss-for-dense-object-detection)
  - ロス計算(CE)をサンプルの難易度によって動的に変化させるFocal lossを提案
  - ベース構造はFPNのようなピラミッド構成だが、1stageとなっている。
  - シンプルな構成のため現在でもベースラインされることも多い
  - Focal loss自体も現在でも使用されている重要な論文

- [Mask R-CNN 2017/03/20](./cv_003_object_detection/mask_r_cnn.md), 2stage / [論文](https://arxiv.org/abs/1703.06870) / [実装例](https://paperswithcode.com/paper/mask-r-cnn)
  - Instance Segmentation用のheadを追加したMulti-taskモデル
  - 物体検知としては、RoI Poolingの代わりに補完を考慮したRoI Alignを導入

- [RefineDet 2017/11/18](./cv_003_object_detection/refinedet.md), 1stage / [論文](https://arxiv.org/abs/1711.06897) / [実装例](https://paperswithcode.com/paper/single-shot-refinement-neural-network-for)
  - 1stageモデルだが2stageモデルのようにanchor boxをCNNで推定
  - anchor box用CNNと物体検知用のCNNを相互接続し同時に学習
  - 改良のベースはSSDを用いているが、近年参照されることは少ない

- [PANet 2018/03/05](./cv_003_object_detection/panet.md), 2stage / [論文](https://arxiv.org/abs/1803.01534) / [実装例](https://paperswithcode.com/paper/path-aggregation-network-for-instance)
  - buttom-up構造により深い特徴量マップにも高解像情報を付与
  - 後段はMask R-CNNと同様のRoI Alignを使用している
  - RPN側は従来通りのFPN構造と思われる
  - Mask R-CNNと同様、Segmentationタスクにも対応したMulti-taskモデル

- [YOLOv3 2018/04/08](./cv_003_object_detection/yolo_v3.md), 1stage / [論文](https://arxiv.org/abs/1804.02767) / [実装例](https://paperswithcode.com/paper/yolov3-an-incremental-improvement)
  - YOLOv2をベースに改良を実施
  - skip-connectionの採用、複数解像度(3つ)の特徴量の融合など実施
  - anchor box数を増やし、結果として速度面ではYOLOv2よりも低下

- [CornetNet 2018/08/03](./cv_003_object_detection/cornernet.md), 1stage (anchor free) / [論文](https://arxiv.org/abs/1808.01244) / [実装例](https://paperswithcode.com/paper/cornernet-detecting-objects-as-paired)
  - 姿勢推定の影響を受けた技術が多いanchor freeの方式
  - corner poolingによりbounding boxのコーナーをうまく検出している
  - またImageNetなどの事前学習が不要な点も特徴的

- [M2Det 2018/11/12](./cv_003_object_detection/m2det.md), 1stage / [論文](https://arxiv.org/abs/1811.04533) / [実装例](https://paperswithcode.com/paper/m2det-a-single-shot-object-detector-based-on)
  - SSDをベースに特徴量マップを複雑に処理するMLFPNを提案
  - MLFPNはFPNのピラミッド構造を繰り返し可能な設計とした構造
  - 当時は高速・高性能をうたっていたが近年参照されることは少ない論文

- [FCOS 2019/04/02](./cv_003_object_detection/fcos.md), 1stage (anchor free) / [論文](https://arxiv.org/abs/1904.01355) / [実装例](https://paperswithcode.com/paper/fcos-fully-convolutional-one-stage-object)
  - 物体検出をsegmentationと同様画素単位で再構築したanchor freeモデル
  - 特徴量マップの各位置からbounding boxまでの4方向の距離を直接推定する
  - 特徴量マップはFPN構造を使い複数の解像度を使用
  - 応用がしやすいため参照されることが多いanchor freeモデル

- HRNet 2019/04/09, 未調査 / [論文](https://arxiv.org/abs/1904.04514) / [実装例](https://paperswithcode.com/paper/190807919)

- [CenterNet 2019/04/16](./cv_003_object_detection/centernet.md), 1stage (anchor free) / [論文](https://arxiv.org/abs/1904.07850) / [実装例](https://paperswithcode.com/paper/objects-as-points)
  - CornerNetと同様にheatmapを推定するanchor freeモデル
  - CenterNetでは中心位置のheatmapを推定する
  - 高解像な特徴量を扱うための工夫がbackboneに施されている

- [CenterNet 2019/04/17](./cv_003_object_detection/centernet2.md), 1stage (anchor free) / [論文](https://arxiv.org/abs/1904.08189) / [実装例](https://paperswithcode.com/paper/centernet-object-detection-with-keypoint)
  - CenterNetは同名のものが２つあるがこちらはCornerNetの改良版
  - ConerNetはbounding boxの内部を見ていないため誤検出が多い
  - 改善するためにCenter heatmapを計算し、Corner Poolingも改良

- [EfficientDet 2019/11/20](./cv_003_object_detection/efficientdet.md), 1stage / [論文](https://arxiv.org/abs/1911.09070) / [実装例](https://paperswithcode.com/paper/efficientdet-scalable-and-efficient-object)
  - EfficientNetの考え方を取り入れた複合スケール方式を提案
  - backbone自体もEfficientNetを使用
  - FPN系のアーキテクチャの体系を整理しより最適でスケーラブルなBiFPNを提案
  - Focal Lossを用いるなどベースの部分はRetinaNetに近いと思われる

- [YOLOv4 2020/04/23](./cv_003_object_detection/yolo_v4.md), 1stage / [論文](https://arxiv.org/abs/2004.10934) / [実装例](https://paperswithcode.com/paper/yolov4-optimal-speed-and-accuracy-of-object)
  - 研究成果を体系的に整理し高速かつ高精度なYOLOを目指した改良版
  - GPU1個で学習可能でリアルタイム推論が可能なモデルを構築

- [DETR 2020/05/26](./cv_003_object_detection/detr.md) 1stage (anchor free) / [論文](https://arxiv.org/abs/2005.12872) / [実装例](https://paperswithcode.com/paper/end-to-end-object-detection-with-transformers)
  - Transformerを物体検知に導入したモデル
  - backboneの後段の処理としてTransformerを使用
  - これによりanchor boxやNMSなどハンドメイドな部分を削除
  - 正解の割り当てを最適割当問題とみなしhungarian algorithmで解く
  - 比較対象がFaster R-CNNなのでこれからの発展に期待

- [YOLOv5 2020/06/01](./cv_003_object_detection/yolo_v5.md), 1stage / 論文なし / [実装例](https://github.com/ultralytics/yolov5)
  - フレームワークが優れており、広く利用されている物体検知のモデル
  - アーキテクチャの詳細は論文がないため確認が困難
  - v4との比較について第三者が検証しているが明確な結論はでていない

- PP-YOLO 2020/07/23, 未調査 / [論文](https://arxiv.org/abs/2007.12099) / [実装例](https://paperswithcode.com/paper/pp-yolo-an-effective-and-efficient)

- [PSS 2021/01/28](./cv_003_object_detection/pss.md), 1stage (anchor free) / [論文](https://arxiv.org/abs/2101.11782) / [実装例](https://paperswithcode.com/paper/object-detection-made-simpler-by-eliminating)
  - NMSフリーなモデルを学習するためにPSSヘッドを導入した論文
  - PSSヘッドにより1つの正解に対して1つだけ正例を出力するよう学習
  - YOLOXのNMSフリー用オプションとして使用

- [YOLOF 2021/03/17](./cv_003_object_detection/yolo_f.md), 1stage / [論文](https://arxiv.org/abs/2103.09460) / [実装例](https://paperswithcode.com/paper/you-only-look-one-level-feature)
  - FPN構造の成功は複数解像度の特徴量ではなく分割統治法である
  - そのことから単一解像度の特徴量からのモデルを考案した
  - 名前はYOLOが付いているが、ベースはRetinaNetの構造

- Swin Transformer 2021/03/25, 未調査 / [論文](https://arxiv.org/abs/2103.14030) / [実装例](https://paperswithcode.com/paper/swin-transformer-hierarchical-vision)

- [OTA 2021/03/26](./cv_003_object_detection/ota.md), 1stage / [論文](https://arxiv.org/abs/2103.14259) / [実装例](https://paperswithcode.com/paper/ota-optimal-transport-assignment-for-object)
  - ラベル割り当てを最適輸送問題(OTA)として解いたモデル
  - OTAはペアのロスを定義することに加え、需要量と供給量を定義
  - この問題設定かでSinkhorn-Knopp法で最適値を求める
  - YOLOXでこれの高速な手法が用いられている

- PP-YOLOv2 2021/04/21, 未調査 / [論文](https://arxiv.org/abs/2104.10419) / [実装例](https://paperswithcode.com/paper/pp-yolov2-a-practical-object-detector)

- [YOLOX 2021/07/18](./cv_003_object_detection/yolo_x.md), 1stage (anchor free) / [論文](https://arxiv.org/abs/2107.08430) / [実装例](https://paperswithcode.com/paper/yolox-exceeding-yolo-series-in-2021)
  - YOLOに先端のラベル割り当て(OTA)やアンカーレス方式を導入した
  - OTAをより高速にするため近似計算するSimOTAを用いている
  - アンカーレスの方式はシンプルでYOLOv1に近い方式
  - YOLOv3をベースとしているため、特徴量融合もv3の方式となる
  - YOLOシリーズ特有のSingle HeadはDecoupledされている

- YOLOP 2021/08/25, 未調査 / [論文](https://arxiv.org/abs/2108.11250) / [実装例](https://paperswithcode.com/paper/yolop-you-only-look-once-for-panoptic-driving)

- PP-PicoDet 2021/11/01, 未調査 / [論文](https://arxiv.org/abs/2111.00902) / [実装例](https://paperswithcode.com/paper/pp-picodet-a-better-real-time-object-detector)

- PP-YOLOE 2022/03/30, 未調査 / [論文](https://arxiv.org/abs/2203.16250) / [実装例](https://paperswithcode.com/paper/pp-yoloe-an-evolved-version-of-yolo)

- ViTDet 2022/03/30, 未調査 / [論文](https://arxiv.org/abs/2203.16527) / [実装例](https://paperswithcode.com/paper/exploring-plain-vision-transformer-backbones)


## 参考

- 物体検出の概要がわかる
  - https://www.youtube.com/watch?v=5nmVHoA-A2E

- 物体検出についての歴史まとめ
  - https://qiita.com/mshinoda88/items/9770ee671ea27f2c81a9
  - https://qiita.com/mshinoda88/items/c7e0967923e3ed47fee5

- 物体検出モデルの進展
  - https://qiita.com/TaigaHasegawa/items/b05110a2571a5289cbab
  - https://qiita.com/TaigaHasegawa/items/a3cb98fb27cc7a9307b4
  - https://qiita.com/TaigaHasegawa/items/653abc81ac4ee1f0d7b8

- YOLOシリーズ徹底解説
  - https://deepsquare.jp/2020/09/yolo/

- ディープラーニングによる一般物体検出アルゴリズムまとめ </td><td> NegativeMindException
  - https://blog.negativemind.com/portfolio/deep-learning-generic-object-detection-algorithm/

- YOLOのv1～v5まで
  - https://qiita.com/tfukumori/items/519d84bf3feb8d246924

- YOLOシリーズについて2021年時点までの情報がいろいろ書いてある。
  - https://www.slideshare.net/ren4yu/you-only-look-onelevel-feature

- 2014～2019までの物体検知の歴史
  - https://github.com/hoya012/deep_learning_object_detection
