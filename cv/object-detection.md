# Object detection

## 概要

* 物体検出とはclassificationに加えて、localizationが必要なタスクである。
* 物体を囲む矩形領域をbounding boxと呼ぶ。
* 物体検出は以下を予測値として学習する。
  * classification
  * 物体の中心座標x,y(連続値)
  * bounding boxのheightとwidth(連続値)
* 学習の際、正解がない場所についてはback propagationをしないなど工夫をして学習が必要。
* 上記の基本的な手法のままでは、以下の課題がある
  * Convの最終層の画像サイズに依存した個数しか検出できない。(最終層が8x8ならば64個が上限)
  * 中心が同じ領域にあるものを検出できない。(学習時の正解も一つしか設定できない)
* 上記を解決するために、anchor boxを準備する。
  * anchor boxはいろんなサイズ、縦横比のbounding box候補のようなイメージ。
  * anchor boxの種類毎に、上記の基本的な手法で学習を実施する。
  * anchor boxの種類はクラスタリングなどにより求める。
  * anchor boxの種類毎に、最終層の出力解像度を分けるアプローチのモデルもある。
* NMS(Non-maximum Supressions)で候補を削減。
  * anchor boxを用いると、重なりの大きな物体が複数検出されるので、IoUが一定以上の場合は、その中で一番大きいスコアの領域のみを採用する。
* これらの手順は煩雑なので、anchor boxを使わない方法も近年人気である。
  * 最終層を元画像の1/4程度に収めて、高解像で出力するCenterNetなど。

## 関連学会

* CVPR
* ICCV
* NeurlPS

## レポジトリ(フレームワーク)

* まとめ
  * https://aru47.hatenablog.com/entry/2022/01/01/123929
* [mmdetection](https://github.com/open-mmlab/mmdetection)
  * OpenMMLabによるレポジトリ
* [PaddleDetection](https://github.com/PaddlePaddle/PaddleDetection)
  * Baidu社によるPaddlePaddleというフレームワークを用いたレポジトリ
* [detectron2](https://github.com/facebookresearch/detectron2)
  * FAIRによるレポジトリ

## 主要なモデル一覧

| 名前               | 発表年月日      | サマリ                                                                                                                                                 | カテゴリ                           | backbone                            | リンク                                                                                                                                                                           |   |
| ---------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| HOG+SVM          | 2005/06/20 | <p>・CNN誕生前のモデルで良く参照された論文<br>・HOGはpixel変化を捉える特徴量<br>・HOG特徴量を用いてSVMで多クラス分類する<br>・NMSはこの時から使われている</p>                                                  | None                           | None                                | <p>解説<br><a href="http://lear.inrialpes.fr/people/triggs/pubs/Dalal-cvpr05.pdf">論文</a></p>                                                                                    |   |
| R-CNN            | 2013/11/11 | <p>・CNN適用の先駆け論文<br>・selective searchという古典的手法で領域候補を抽出<br>・候補領域をリサイズしてCNNに入力して特徴量ベクトルを得る<br>・その後段で1-class SVMとbounding boxのregressionを実施</p>          | 2stage                         | <p>AlexNet<br>VGG16</p>             | <p>解説<br><a href="https://arxiv.org/abs/1311.2524">論文</a><br><a href="https://paperswithcode.com/paper/rich-feature-hierarchies-for-accurate-object">paperswithcode</a></p>   |   |
| SPP-net          | 2014/06/18 | <p>・領域候補の切り出しを特徴量マップに対して実施し効率化<br>・これにより最大2000毎程度ある領域候補のCNN処理が1回で実現可能<br>・切り出した特徴量マップはSPPで固定長の特徴量に変換して後段で処理</p>                                     | 2stage                         | <p>ZFNet<br>AlexNet<br>OverFeat</p> | <p>解説<br><a href="https://arxiv.org/abs/1406.4729">論文</a><br><a href="https://paperswithcode.com/paper/spatial-pyramid-pooling-in-deep-convolutional">paperswithcode</a></p>  |   |
| Fast R-CNN       | 2015/04/30 | <p>・Multi-task lossによりclassificationとbounding box推定を同時学習<br>・固定長の特徴量変換としてSPPの代わりにRoI Poolingを使用<br>・RoI Poolingは両機をグリッド分割し各グリッドに対してpooling処理を実施</p> | 2stage                         | VGG16                               | <p>解説<br><a href="https://arxiv.org/abs/1504.08083">論文<br></a><a href="https://paperswithcode.com/paper/fast-r-cnn">paperswithcode</a></p>                                    |   |
| Faster R-CNN     | 2015/06/04 | <p>・領域候補を抽出するselective searchをCNNで置き換え<br>・このNetworkをRPN(Region Proposal Network)と呼ぶ<br>・RPNには9個のanchor boxを使用し領域有無とbounding box回帰を実施</p>           | 2stage                         | <p>VGG16<br>ResNet101</p>           | <p>解説<br><a href="https://arxiv.org/abs/1506.01497">論文</a><br><a href="https://paperswithcode.com/paper/faster-r-cnn-towards-real-time-object">paperswithcode</a></p>         |   |
| YOLO             | 2015/06/08 | <p>・領域検出とclassificationの同時学習を実現(You only look once)<br>・入力画像をグリッド分割し、各グリッドに固定数のbounding box回帰を実施</p>                                                | <p>1stage<br>(anchor free)</p> | 独自実装                                | <p>解説<br><a href="https://arxiv.org/abs/1506.02640">論文</a><br><a href="https://paperswithcode.com/paper/you-only-look-once-unified-real-time-object">paperswithcode</a></p>   |   |
| SSD              | 2015/12/08 | <p>・YOLOと同じく領域検出とclassificationの同時学習を実現(Single shot detector)<br>・backboneの途中層の高解像な特徴量マップを使用<br>・特徴量マップの解像度毎に様々な大きさのanchor boxを定義</p>               | <p>1stage<br>(anchor box)</p>  | VGG16                               | <p>解説<br><a href="https://arxiv.org/abs/1512.02325">論文</a><br><a href="https://paperswithcode.com/paper/ssd-single-shot-multibox-detector">paperswithcode</a></p>             |   |
| R-FCN            | 2016/05/20 | <p>・RoI Pooling以降の全結合層をCNN化したモデル<br>・RoI Poolingの前段にposition sensitive mapという畳み込みを追加<br>・その後段でRoI Poolingを実施する構成</p>                                | 2stage                         | ResNet-101                          | <p>解説<br><a href="https://arxiv.org/abs/1605.06409">論文</a><br><a href="https://paperswithcode.com/paper/r-fcn-object-detection-via-region-based-fully">paperswithcode</a></p> |   |
| FPN              | 2016/12/09 | <p>・ピラミッド構造の特徴量マップを実現したモデル<br>・ベースはFaster R-CNNであり、RPNとclassificationどちらもピラミッド構造化<br>・FPNの考え方は現在まで採用されており重要な論文</p>                                  | 2stage                         | <p>ResNet-50<br>ResNet-101</p>      | <p>解説<br><a href="https://arxiv.org/abs/1612.03144">論文</a><br><a href="https://paperswithcode.com/paper/feature-pyramid-networks-for-object-detection">paperswithcode</a></p> |   |
| YOLOv2(YOLO9000) | 2016/12/25 | <p>・YOLOをベースに改良を実施<br>・入力の高解像化、anchor boxの導入、高解像な特徴量マップの使用など<br>・それ以外に9000クラスまで分類可能なWordTreeというアーキテクチャを構築</p>                                       | <p>1stage<br>(anchor box)</p>  | Darknet-19                          | <p>解説<br><a href="https://arxiv.org/abs/1612.08242">論文</a><br><a href="https://paperswithcode.com/paper/yolo9000-better-faster-stronger">paperswithcode</a></p>               |   |

1stage\
(anchor box)Darknet-19\[解説]\()\[論文]\()\[公式(Darknet)]\(https://pjreddie.com/darknet/yolo)\
\[paperswithcode]\() RetinaNet2017/08/07・ロス計算(CE)をサンプルの難易度によって動的に変化させるFocal lossを提案\
・ベース構造はFPNのようなピラミッド構成だが、1stageとなっている。\
・シンプルな構成のため現在でもベースラインされることも多い\
・Focal loss自体も現在でも使用されている重要な論文1stage\
(anchor box)ResNet-50\
ResNet-101\[解説]\(./cv\_003\_object\_detection/retinanet.md)\[論文]\(https://arxiv.org/abs/1708.02002)\[公式(detectron)]\(https://github.com/facebookresearch/Detectron)\
\[paperswithcode]\(https://paperswithcode.com/paper/focal-loss-for-dense-object-detection) Mask R-CNN2017/03/20・Instance Segmentation用のheadを追加したMulti-taskモデル\
・物体検知としては、RoI Poolingの代わりに補完を考慮したRoI Alignを導入2stageResNet-50\
ResNet-101\
ResNeXt\[解説]\(./cv\_003\_object\_detection/mask\_r\_cnn.md)\[論文]\(https://arxiv.org/abs/1703.06870)\[公式(tensorflow)]\(https://github.com/matterport/Mask\_RCNN)\
\[paperswithcode]\(https://paperswithcode.com/paper/mask-r-cnn) RefineDet2017/11/18・1stageモデルだが2stageモデルのようにanchor boxをCNNで推定\
・anchor box用CNNと物体検知用のCNNを相互接続し同時に学習\
・改良のベースはSSDを用いているが、近年参照されることは少ない1stage\
(anchor free) VGG16\
ResNet-101\[解説]\(./cv\_003\_object\_detection/refinedet.md)\[論文]\(https://arxiv.org/abs/1711.06897)\[公式(Caffe)]\(https://github.com/sfzhang15/RefineDet)\
\[paperswithcode]\(https://paperswithcode.com/paper/single-shot-refinement-neural-network-for) PANet2018/03/05・buttom-up構造により深い特徴量マップにも高解像情報を付与\
・後段はMask R-CNNと同様のRoI Alignを使用している\
・RPN側は従来通りのFPN構造と思われる\
・Mask R-CNNと同様、Segmentationタスクにも対応したMulti-taskモデル2stageResNet-50\
ResNeXt-101\[解説]\(./cv\_003\_object\_detection/panet.md)\[論文]\(https://arxiv.org/abs/1803.01534)\[公式(PyTorch)]\(https://github.com/ShuLiu1993/PANet)\
\[paperswithcode]\(https://paperswithcode.com/paper/path-aggregation-network-for-instance) YOLOv32018/04/08・YOLOv2をベースに改良を実施\
・skip-connectionの採用、複数解像度(3つ)の特徴量の融合など実施\
・anchor box数を増やし、結果として速度面ではYOLOv2よりも低下1stage\
(anchor box)Darknet-53\[解説]\(./cv\_003\_object\_detection/yolo\_v3.md)\[論文]\(https://arxiv.org/abs/1804.02767)\[公式(Darknet)]\(https://pjreddie.com/darknet/yolo/)\
\[paperswithcode]\(https://paperswithcode.com/paper/yolov3-an-incremental-improvement) CornetNet2018/08/03・姿勢推定の影響を受けた技術が多いanchor freeの方式\
・corner poolingによりbounding boxのコーナーをうまく検出している\
・またImageNetなどの事前学習が不要な点も特徴的1stage\
(anchor free)Hourglass104\[解説]\(./cv\_003\_object\_detection/cornernet.md)\[論文]\(https://arxiv.org/abs/1808.01244)\[公式(PyTorch)]\(https://github.com/princeton-vl/CornerNet)\
\[paperswithcode]\(https://paperswithcode.com/paper/cornernet-detecting-objects-as-paired) M2Det2018/11/12・SSDをベースに特徴量マップを複雑に処理するMLFPNを提案\
・MLFPNはFPNのピラミッド構造を繰り返し可能な設計とした構造\
・当時は高速・高性能をうたっていたが近年参照されることは少ない論文1stage\
(anchor box)VGG16\
ResNet-101\[解説]\(./cv\_003\_object\_detection/m2det.md)\[論文]\(https://arxiv.org/abs/1811.04533)\[公式(PyTorch)]\(https://github.com/VDIGPKU/M2Det)\
\[paperswithcode]\(https://paperswithcode.com/paper/m2det-a-single-shot-object-detector-based-on) FCOS2019/04/02・物体検出をsegmentationと同様画素単位で再構築したanchor freeモデル\
・特徴量マップの各位置からbounding boxまでの4方向の距離を直接推定する\
・特徴量マップはFPN構造を使い複数の解像度を使用\
・応用がしやすいため参照されることが多いanchor freeモデル1stage\
(anchor free)ResNet-101\
HRNet\
ResNeXt-101\[解説]\(./cv\_003\_object\_detection/fcos.md)\[論文]\(https://arxiv.org/abs/1904.01355)\[公式(PyTorch)]\(https://github.com/tianzhi0549/FCOS)\
\[paperswithcode]\(https://paperswithcode.com/paper/fcos-fully-convolutional-one-stage-object) CenterNet2019/04/16・CornerNetと同様にheatmapを推定するanchor freeモデル\
・CenterNetでは中心位置のheatmapを推定する\
・高解像な特徴量を扱うための工夫がbackboneに施されている1stage\
(anchor free)ResNet-18\
ResNet-101\
DLA-34\
Hourglass-104\[解説]\(./cv\_003\_object\_detection/centernet.md)\[論文]\(https://arxiv.org/abs/1904.07850)\[公式(PyTorch)]\(https://github.com/xingyizhou/CenterNet)\
\[paperswithcode]\(https://paperswithcode.com/paper/objects-as-points) CenterNet2019/04/17・CenterNetは同名のものが２つあるがこちらはCornerNetの改良版\
・ConerNetはbounding boxの内部を見ていないため誤検出が多い\
・改善するためにCenter heatmapを計算し、Corner Poolingも改良1stage\
(anchor free)Hourglass-52\
Hourglass-104\[解説]\(./cv\_003\_object\_detection/centernet2.md)\[論文]\(https://arxiv.org/abs/1904.08189)\[公式(PyTorch)]\(https://github.com/Duankaiwen/CenterNet)\
\[paperswithcode]\(https://paperswithcode.com/paper/centernet-object-detection-with-keypoint) EfficientDet2019/11/20・EfficientNetの考え方を取り入れた複合スケール方式を提案\
・backbone自体もEfficientNetを使用\
・FPN系のアーキテクチャの体系を整理しより最適でスケーラブルなBiFPNを提案\
・Focal Lossを用いるなどベースの部分はRetinaNetに近いと思われる1stage\
(anchor box)EfficientNet\[解説]\(./cv\_003\_object\_detection/efficientdet.md)\[論文]\(https://arxiv.org/abs/1911.09070)\[公式(tensorflow)]\(https://github.com/google/automl)\
\[paperswithcode]\(https://paperswithcode.com/paper/efficientdet-scalable-and-efficient-object) YOLOv42020/04/23・研究成果を体系的に整理し高速かつ高精度なYOLOを目指した改良版\
・GPU1個で学習可能でリアルタイム推論が可能なモデルを構築1stage\
(anchor box)CSPDarknet-53\[解説]\(./cv\_003\_object\_detection/yolo\_v4.md)\[論文]\(https://arxiv.org/abs/2004.10934)\[公式(tensorflow)]\(https://github.com/AlexeyAB/darknet)\
\[paperswithcode]\(https://paperswithcode.com/paper/yolov4-optimal-speed-and-accuracy-of-object) DETR2020/05/26・Transformerを物体検知に導入したモデル\
・backboneの後段の処理としてTransformerを使用\
・これによりanchor boxやNMSなどハンドメイドな部分を削除\
・正解の割り当てを最適割当問題とみなしhungarian algorithmで解く\
・比較対象がFaster R-CNNなのでこれからの発展に期待1stage\
(anchor free)\
TransformerResNet-50\
ResNet-101\[解説]\(./cv\_003\_object\_detection/detr.md)\[論文]\(https://arxiv.org/abs/2005.12872)\[公式(PyTorch)]\(https://github.com/facebookresearch/detr)\
\[paperswithcode]\(https://paperswithcode.com/paper/end-to-end-object-detection-with-transformers) YOLOv52020/06/01・フレームワークが優れており、広く利用されている物体検知のモデル\
・アーキテクチャの詳細は論文がないため確認が困難\
・v4との比較について第三者が検証しているが明確な結論はでていない1stage\
(ancor box)???\[解説]\(./cv\_003\_object\_detection/yolo\_v5.md)なし\[公式(PyTorch)]\(https://github.com/ultralytics/yolov5) PP-YOLO2020/07/23未調査未調査未調査未調査\[論文]\(https://arxiv.org/abs/2007.12099)\[公式(Paddle)]\(https://github.com/PaddlePaddle/PaddleDetection)\
\[paperswithcode]\(https://paperswithcode.com/paper/pp-yolo-an-effective-and-efficient) PSS2021/01/28WIPWIPWIP\[解説]\(./cv\_003\_object\_detection/pss.md)\[論文]\(https://arxiv.org/abs/2101.11782)\[公式(PyTorch)]\(https://github.com/damo-cv/FCOSPss)\
\[paperswithcode]\(https://paperswithcode.com/paper/object-detection-made-simpler-by-eliminating) YOLOF2021/03/17WIPWIPResNet-50\
ResNet-101\[解説]\(./cv\_003\_object\_detection/yolo\_f.md)\[論文]\(https://arxiv.org/abs/2103.09460)\[公式(PyTorch)]\(https://github.com/megvii-model/YOLOF)\
\[paperswithcode]\(https://paperswithcode.com/paper/you-only-look-one-level-feature) Swin Transformer2021/03/25未調査未調査未調査未調査\[論文]\(https://arxiv.org/abs/2103.14030)\[公式(PyTorch)]\(https://github.com/SwinTransformer/Swin-Transformer-Object-Detection)\
\[paperswithcode]\(https://paperswithcode.com/paper/swin-transformer-hierarchical-vision) OTA2021/03/26WIPWIPWIP\[解説]\(./cv\_003\_object\_detection/ota.md)\[論文]\(https://arxiv.org/abs/2103.14259)\[公式(PyTorch)]\(https://github.com/Megvii-BaseDetection/OTA)\
\[paperswithcode]\(https://paperswithcode.com/paper/ota-optimal-transport-assignment-for-object) PP-YOLOv22021/04/21未調査未調査未調査未調査\[論文]\(https://arxiv.org/abs/2104.10419)\[公式(Paddle)]\(https://github.com/PaddlePaddle/PaddleDetection)\
\[paperswithcode]\(https://paperswithcode.com/paper/pp-yolov2-a-practical-object-detector) YOLOX2021/07/18WIPWIPWIP\[解説]\(./cv\_003\_object\_detection/yolo\_x.md)\[論文]\(https://arxiv.org/abs/2107.08430)\[公式(PyTorch)]\(https://github.com/Megvii-BaseDetection/YOLOX)\
\[paperswithcode]\(https://paperswithcode.com/paper/yolox-exceeding-yolo-series-in-2021) YOLOP2021/08/25未調査未調査未調査未調査\[論文]\(https://arxiv.org/abs/2108.11250)\[公式(PyTorch)]\(https://github.com/hustvl/yolop)\
\[paperswithcode]\(https://paperswithcode.com/paper/yolop-you-only-look-once-for-panoptic-driving) PP-PicoDet2021/11/01未調査未調査未調査未調査\[論文]\(https://arxiv.org/abs/2111.00902)\[公式(Paddle)]\(https://github.com/PaddlePaddle/PaddleDetection)\
\[paperswithcode]\(https://paperswithcode.com/paper/pp-picodet-a-better-real-time-object-detector) PP-YOLOE2022/03/30未調査未調査未調査未調査\[論文]\(https://arxiv.org/abs/2203.16250)\[公式(Paddle)]\(https://github.com/PaddlePaddle/PaddleDetection)\
\[paperswithcode]\(https://paperswithcode.com/paper/pp-yoloe-an-evolved-version-of-yolo) ViTDet2022/03/30未調査未調査未調査未調査\[論文]\(https://arxiv.org/abs/2203.16527)\[公式(PyTorch)]\(https://github.com/ViTAE-Transformer/ViTDet)\
\[paperswithcode]\(https://paperswithcode.com/paper/exploring-plain-vision-transformer-backbones)

## 参考

* 物体検出の概要がわかる
  * https://www.youtube.com/watch?v=5nmVHoA-A2E
* 物体検出についての歴史まとめ
  * https://qiita.com/mshinoda88/items/9770ee671ea27f2c81a9
  * https://qiita.com/mshinoda88/items/c7e0967923e3ed47fee5
* 物体検出モデルの進展
  * https://qiita.com/TaigaHasegawa/items/b05110a2571a5289cbab
  * https://qiita.com/TaigaHasegawa/items/a3cb98fb27cc7a9307b4
  * https://qiita.com/TaigaHasegawa/items/653abc81ac4ee1f0d7b8
* YOLOシリーズ徹底解説
  * https://deepsquare.jp/2020/09/yolo/
* ディープラーニングによる一般物体検出アルゴリズムまとめ NegativeMindException
  * https://blog.negativemind.com/portfolio/deep-learning-generic-object-detection-algorithm/
* YOLOのv1～v5まで
  * https://qiita.com/tfukumori/items/519d84bf3feb8d246924
* YOLOシリーズについて2021年時点までの情報がいろいろ書いてある。
  * https://www.slideshare.net/ren4yu/you-only-look-onelevel-feature
* 2014～2019までの物体検知の歴史
  * https://github.com/hoya012/deep\_learning\_object\_detection
