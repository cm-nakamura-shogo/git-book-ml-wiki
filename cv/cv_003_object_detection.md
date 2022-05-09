# Object detection

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

## 関連学会

- CVPR
- ICCV
- NeurlPS

## レポジトリ(フレームワーク)

- まとめ
  - https://aru47.hatenablog.com/entry/2022/01/01/123929

- [mmdetection](https://github.com/open-mmlab/mmdetection)
  - OpenMMLabによるレポジトリ
- [PaddleDetection](https://github.com/PaddlePaddle/PaddleDetection)
  - Baidu社によるPaddlePaddleというフレームワークを用いたレポジトリ
- [detectron2](https://github.com/facebookresearch/detectron2)
  - FAIRによるレポジトリ

## 主要なモデル一覧

<table>
  <thead><tr><th>名前</th><th>発表年月日</th><th>サマリ</th><th>カテゴリ</th><th>backbone</th><th>リンク</th></tr></thead>
  <tbody>
    <tr>
      <td>HOG+SVM</td><td>2005/06/20</td>
      <td>
        ・CNN誕生前のモデルで良く参照された論文<br>
        ・HOGはpixel変化を捉える特徴量<br>
        ・HOG特徴量を用いてSVMで多クラス分類する<br>
        ・NMSはこの時から使われている
      </td>
      <td>None</td><td>None</td>
      <td>
        <a href="./cv_003_object_detection/hog_svm.md">解説</a><br>
        <a href="http://lear.inrialpes.fr/people/triggs/pubs/Dalal-cvpr05.pdf">論文</a>
      </td>
    </tr>
  </tbody>
</table>

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
